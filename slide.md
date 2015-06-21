Pitch Presentation
========================================================
author: Marius
date: 

Goal of the App
========================================================
This app build a linear model using Motor Trend Car Road Tests (mtcars):
- Outcome is mpg (Miles/(US) gallon)
- Predictor is one of the other variable
How it works
========================================================
- Choose the predictor 

- Then the app draw scatterplot mpg **vs** one predictor 

- And a red line, which is a linear model estimated using the data

- evaluate the slop
ui.R code
========================================================


```r
library(shiny)
shinyUI(fluidPage(
    title = 'Download a PDF report',
    sidebarLayout(
        sidebarPanel(
            helpText(),
            selectInput('x', 'This app build a regression model of mpg against:',
                        choices = names(mtcars)[-1])
            #textInput("y","")
        ),
        mainPanel(
            plotOutput('regPlot'),
            h4("The slop is"),
            verbatimTextOutput("inputValue")
        )
    )
))
```

Server.R code
========================================================

```r
shinyServer(function(input, output) {
    
    regFormula <- reactive({
        as.formula(paste('mpg ~', input$x))
    })
    
    output$regPlot <- renderPlot({
        par(mar = c(4, 4, .1, .1))
        plot(regFormula(), data = mtcars, pch = 18)
        abline(lm(regFormula(),data=mtcars),col=2)
    })
    output$inputValue= renderPrint({lm(regFormula(),data=mtcars)$coef[2] })
    
})
```
