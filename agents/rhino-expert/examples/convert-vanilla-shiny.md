# Example: Converting Vanilla Shiny to Rhino

## Input (Vanilla Shiny)

User provides this code:

```r
# app.R
library(shiny)
library(dplyr)
source("R/helpers.R")

ui <- fluidPage(
  titlePanel("City Dashboard"),
  selectInput("city", "Select City:", choices = NULL),
  plotOutput("emissions_chart")
)

server <- function(input, output, session) {
  cities <- reactive({
    con <- get_connection()
    dbGetQuery(con, "SELECT * FROM dim_cities")
  })

  observe({
    updateSelectInput(session, "city", choices = cities()$city)
  })

  output$emissions_chart <- renderPlot({
    req(input$city)
    # ... plot code
  })
}

shinyApp(ui, server)
```

## Agent Response

The agent would:

1. **Detect issues:**
   - `library()` calls
   - `source()` statement
   - Non-modular structure
   - Mixed logic and view code

2. **Propose rhino conversion:**

### app/logic/queries.R

```r
box::use(
  DBI[dbGetQuery],
)

#' Get all cities from database
#'
#' @param con Database connection
#' @return tibble of cities
#' @export
get_cities <- function(con) {
  dbGetQuery(con, "SELECT * FROM dim_cities WHERE current_member = TRUE")
}
```

### app/view/city_selector.R

```r
box::use(
  shiny[moduleServer, NS, selectInput, updateSelectInput, reactive, observe],
  app/logic/queries[get_cities],
)

#' City selector UI
#' @param id Module namespace
#' @export
ui <- function(id) {

  ns <- NS(id)
  selectInput(ns("city"), "Select City:", choices = NULL)
}

#' City selector server
#' @param id Module namespace
#' @param con Database connection
#' @return Reactive selected city
#' @export
server <- function(id, con) {
  moduleServer(id, function(input, output, session) {
    cities <- reactive({
      get_cities(con)
    })

    observe({
      updateSelectInput(session, "city", choices = cities()$city)
    })

    reactive(input$city)
  })
}
```

### app/view/emissions_chart.R

```r
box::use(
  shiny[moduleServer, NS, plotOutput, renderPlot, reactive, req],
  app/logic/queries[get_city_emissions],
)

#' Emissions chart UI
#' @param id Module namespace
#' @export
ui <- function(id) {
  ns <- NS(id)
  plotOutput(ns("chart"))
}

#' Emissions chart server
#' @param id Module namespace
#' @param con Database connection
#' @param selected_city Reactive city name
#' @export
server <- function(id, con, selected_city) {
  moduleServer(id, function(input, output, session) {
    output$chart <- renderPlot({
      req(selected_city())
      data <- get_city_emissions(con, selected_city())
      # ... plot code
    })
  })
}
```

### app/main.R

```r
box::use(
  shiny[bootstrapPage, moduleServer, NS],
  bslib[page_fluid],
  app/view/city_selector,
  app/view/emissions_chart,
  app/logic/database[get_pool],
)

#' @export
ui <- function(id) {
 ns <- NS(id)
  page_fluid(
    title = "City Dashboard",
    city_selector$ui(ns("city_select")),
    emissions_chart$ui(ns("emissions"))
  )
}

#' @export
server <- function(id) {
  moduleServer(id, function(input, output, session) {
    con <- get_pool()

    selected_city <- city_selector$server("city_select", con)
    emissions_chart$server("emissions", con, selected_city)
  })
}
```

3. **Update __init__.R files**
4. **Run `rhino::lint_r()`**

## Key Transformations

| Before | After |
|--------|-------|
| `library(shiny)` | `box::use(shiny[...])` |
| `source("R/helpers.R")` | `box::use(app/logic/helpers)` |
| Monolithic app.R | Modular view/ and logic/ |
| Direct server code | `moduleServer()` pattern |
