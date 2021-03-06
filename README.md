# Basics of Plotting Project
### About: 

For this project, I implemented an application using Shiny-R. I used shiny to build the project. I created a simple code to plot datasets the user picks on the application.

### Note:

Dataset available on the R package.

### Results: 

First, I set a variable called *datasets* to contain the data.

```R
datasets <- c("economics", "faithfuld", "seals")
```
  
Afterwards, I created the UI (User Interface) for the shiny application with a *fluidPage* layout. 

```R
ui <- fluidPage(
  selectInput("dataset", "Dataset", choices = datasets),
  verbatimTextOutput("summary"),
  plotOutput("plot")
)
```
  
The UI is similar to the ingredients in a recipe. So lets list out the ingredients and what they do.
 
*SelectInput* lets the user choise the data set they want to see.

*verbatimTextOutput* displays the code and will later help me print the data.

*plotOutput* displays the code and will later help me plot the data.

Now lets focus on the server. The server is the directions in the recipe. The ingredients will be used as the directions describes them.

```R

server <- function(input, output, session) {
  dataset <- reactive({
    get(input$dataset, "package:ggplot2")
  })
  output$summary <- renderPrint({
    summary(dataset())
  })
  output$plot <- renderPlot({
    plot(dataset)
  }, res = 96)
}
shinyApp(ui, server)
```

So lets list out the directions and what they do.

First, I created a *reactive*. Reactive expressions let me control which parts of my app update when, which prevents unnecessary computation that can slow down my app. In this case, it optimizes the code by having less code written which increases the apps speed. 

*ouput$summary* variable is set to *renderPrint* which is paired with *verbatimTextOutput*. This is because the server side of the code uses a specific render function to wrap the code that is provided. 

Each render{Type} function is designed to produce a particular type of output (e.g. text, tables, and plots), and is often paired with a {type}Output function. 

In this case, the *renderPrint* prints a summary of the dataset.

*output$plot* variable is set to *renderPlot* which is paried with *plotOutput*. The *plotOutput* plots the dataset. The resolution is usually set to **96** so that the Shiny plots match what I see in RStudio as closely as possible.
 
Finally, I write down the code *shinyApp(ui, server)* which constructs and starts the Shiny application from UI and server.
 
### Final Product: 

<img width="1261" alt="Screen Shot 2021-09-24 at 5 39 03 PM" src="https://user-images.githubusercontent.com/89553126/134746506-5bdd6eb1-d4b6-4d07-ae0b-29abe56683f0.png">

<img width="1261" alt="Screen Shot 2021-09-24 at 5 39 10 PM" src="https://user-images.githubusercontent.com/89553126/134746509-9cc8c9b6-6295-42ed-8e1b-911cc4750e23.png">

<img width="1261" alt="Screen Shot 2021-09-24 at 5 39 16 PM" src="https://user-images.githubusercontent.com/89553126/134746513-aa330413-bbf0-4520-9ba9-0ba979c758dd.png">
