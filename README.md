Developing-Data-Products2

The basic parts of a Shiny app
ADDED: 12/27/2015

The following program is to utilize very basic functions of Shiny to create plots based on given data/input.
Here is a small program that plots R’s built-in faithful dataset with a configurable number of bins. 

To run the example, type:

> library(shiny)
> runExample("01_hello")

Shiny applications have two components: a user-interface definition and a server script. The source code for both of these components is listed below.

In subsequent sections of the article we’ll break down Shiny code in detail and explain the use of “reactive” expressions for generating output. For now, though, just try playing with the sample application and reviewing the source code to get an initial feel for things.

Be sure to read the comments carefully.

The user interface is defined in a source file named ui.R:

ui.R

library(shiny)

# Define UI for application that draws a histogram
shinyUI(fluidPage(

  # Application title
  titlePanel("Hello Shiny!"),

  # Sidebar with a slider input for the number of bins
  sidebarLayout(
    sidebarPanel(
      sliderInput("bins",
                  "Number of bins:",
                  min = 1,
                  max = 50,
                  value = 30)
    ),

    # Show a plot of the generated distribution
    mainPanel(
      plotOutput("distPlot")
    )
  )
))




Server Script
The server script declares the datasetInput reactive expression as well as three reactive output values. There are detailed comments for each definition that describe how it works within the reactive system:

server.R

library(shiny)
library(datasets)

  # The output$summary depends on the datasetInput reactive
  # expression, so will be re-executed whenever datasetInput is
  # invalidated
  # (i.e. whenever the input$dataset changes)
  output$summary <- renderPrint({
    dataset <- datasetInput()
    summary(dataset)
  })
  
  # The output$view depends on both the databaseInput reactive
  # expression and input$obs, so will be re-executed whenever
  # input$dataset or input$obs is changed. 
  output$view <- renderTable({
    head(datasetInput(), n = input$obs)
  })
})