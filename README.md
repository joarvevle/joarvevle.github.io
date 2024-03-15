# Learn to make custom ggplot theme packages for R.

This site is to help create a custom ggplot theme for R.  
You will learn to make a theme that can be added to any ggplot and changes the colours, apparance, font and other to reflect your style or organizational requirements.

A great site for extra info can be found here:
https://ourcodingclub.github.io/tutorials/writing-r-package/

# Create a new package

We create this package from a new project. In R studio you find this with File <- New Project  
![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/32e4901b-54a3-468b-ad65-8837234007dd)

Start with New Directory, and choose R Package  
Give your package an appropriate name and choose "Open in new session". 

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/2df4b7a6-f3ee-4412-ab78-74a882d7ba6e)

This will create a new folder names customTheme with a number of files.   

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/b01c17a9-d94a-4181-8b82-8c4a5190d7dc)

We must now add some R files to the package. Your R files should go to the R folder:  

Lets make our first file  
Go to the R folder and create a new R-file with 
> File - New File - R Script  
and call it **custom_colour.R** 


```
.pkgenv <- new.env(parent=emptyenv())

custom =  c("#666EB4", "#4CA2A8", "#AF74B9","#B49566","#DD887C","#E6B056","#DD7CC2","#B0CD5D",
           "#0a6053","#018374","#02BC94","#949494","#595959",
           "#006f79", "#00a6b8","#015183", "#fcb614","#da8220","#837401",
           "#939393","#D0D0D0","#143C36", "#244540", "#334F4B", "#425955",
           "#526260", "#616C6A", "#707675", "#244C46", "#446560", "#637F7A",
           "#013183", "#011083", "#130183", "#330183", "#540183", "#011083",
           "#330183", "#740183", "#830151", "#830110" ,"#0c3205","#2b0532",
           "#32050c","#0c7867","#0A6028","#17600A","#0F8E3B","#0A6053",
           "#0A4053","#0A4260","#0A1760","#0F628E", "#42600A")

#Add two colour schemes for fill and colour function

.pkgenv[["custom_fill_light"]] <- function(){
  scale_fill_manual(values = custom_light )
}

.pkgenv[["custom_colour_light"]] <- function(){
  scale_colour_manual(values = custom_light )
}

```

Also create a new file containing the actual theme  
> File - New File - R Script  
and call it **custom_theme.R** 

```
.pkgenv[["theme_custom_base"]] <- function(legend=0){
  theme_minimal() %+replace%
    theme(
      #change font
      text=element_text(family="gochi"), # need to include package

      #title, axis title and legend title
      plot.title = element_text(color = '#018374',face="bold",size=22, hjust = 0 , vjust = 1),
      axis.title = element_text(color = "#018374",size=14,face="bold"),
      legend.title = element_text(color = '#018374', size = 14, hjust = 0),
      strip.text = element_text(colour = '#018374', size = 14,vjust = .4),

      #subtitle, caption and legend text
      legend.text = element_text(color = '#05322b',size = 14),
      plot.subtitle = element_text(color = "#05322b", hjust = 0, vjust = 0.3),
      plot.caption = element_text(color = "#05322b", hjust = 1),

      #Name for category
      axis.text.x = element_text(color = '#05322b',size = 12,face="bold", hjust = 0.5, angle = legend),
      axis.text.y = element_text(color = '#05322b',size = 12,face="bold"),
      axis.title.x = element_text(hjust = 0.5),
      panel.grid.major.x = element_blank(),

      #Change grid lines to ligth grey
      panel.grid.major.y = element_line(colour="#D0D0D0"),
      panel.grid.minor.y = element_line(colour="#D0D0D0"),

    )

 }
```
To learn how to change the different aspects of your ggplot refere to this page:  
https://ggplot2.tidyverse.org/reference/theme.html  

Now create a new file to create the actual functions that you will use in your plots.  

> File - New File - R Script  
and call it **mainFunction.R** 

```
.pkgenv[["custom_colour"]] <- function(values = custom){
  scale_colour_manual(values = values)
}

.pkgenv[["custom_fill"]] <- function(values = custom){
  scale_fill_manual(values = values)
}

theme_custom <- function(legend=0, palette=custom){
  list(.pkgenv[["theme_custom_base"]](legend=legend), .pkgenv[["custom_colour"]](values = palette), .pkgenv[["custom_fill"]](values = palette))
}

```

> File - New File - R Script  
and call it **zzz.R** 

The final step will add your custom fonts. This is done through the package showtext and sysfonts.

```
.onLoad <- function(libname, pkgname) {
  sysfonts::font_add_google("Gochi Hand", "gochi")
  showtext::showtext_auto()
}
```
Now we should add the man files. This should be located in the **man** folder, and have one file for every function. Below is an example for the cusom pallete.

```
\name{custom}
\alias{custom}
%- Also NEED an '\alias' for EACH other topic documented here.
\title{custom
%%  ~~function to do ... ~~
}
\description{
A vector with colour. This is the standard palette
}
\references{


}
\author{
your_name
}

\examples{

library(customTheme)
library(tidyverse)

starwars %>%
  head(23) %>%
  ggplot()+
  geom_col(aes(homeworld,height, fill=skin_color), position = "dodge")+
  labs(title = "Age in starwars", subtitle = "for different planets and eye colour", caption = "DataSource : Starwars")+
  theme_custom(legend = 25)
  }

```






  


