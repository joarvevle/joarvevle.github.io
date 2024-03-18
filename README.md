# Make custom ggplot theme packages for R, with custom colour, fonts and appearance

This site is to help create a custom ggplot theme package for R.  
If you follow this guide you should in 20 minutes have your own R package that can be added to any ggplot and changes the colours, appearance, font and other to reflect your style or organizational requirements.

A great site for extra info can be found here:
https://ourcodingclub.github.io/tutorials/writing-r-package/  
and here  
https://medium.com/@abertozz/turning-an-r-package-into-a-github-repo-aeaebacfe1c  

Here is an exemple of the plot that this tutorial will help you make.   

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/ccdbc7b5-32e8-4e28-8d83-38f1a5af78e1)


Before you start you should install devtools  

```
install.packages("devtools")

```

And update Rstudio:
Help <- Check for updates  
![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/303ec7d0-f787-43ce-a3e6-8d69030f79cf)

Also make sure R is updated to latest version. You could do that on [CRAN](https://cran.r-project.org/) or with installr:

```
install.packages("installr")
library(installr)
installr()
```

And you also need a github account <- [GitHub](https://github.com/)


# Create a new package

We create this package from a new project. In R studio you find this with File <- New Project  
![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/32e4901b-54a3-468b-ad65-8837234007dd)

Start with New Directory, and choose R Package  
Give your package an appropriate name and choose "Open in new session". 

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/2df4b7a6-f3ee-4412-ab78-74a882d7ba6e)

This will create a new folder named customTheme with a number of files.   

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/b01c17a9-d94a-4181-8b82-8c4a5190d7dc)

# add files for the code

We must now add some R files to the package. Your R files should go to the R folder:  

Lets make our first file  
Go to the R folder and create a new R-file with 
> File - New File - R Script  
and call it **custom_colour.R** 

```
custom =  c("#666EB4", "#4CA2A8", "#AF74B9","#B49566","#DD887C","#E6B056","#DD7CC2","#B0CD5D",
           "#0a6053","#018374","#02BC94","#949494","#595959",
           "#006f79", "#00a6b8","#015183", "#fcb614","#da8220","#837401",
           "#939393","#D0D0D0","#143C36", "#244540", "#334F4B", "#425955",
           "#526260", "#616C6A", "#707675", "#244C46", "#446560", "#637F7A",
           "#013183", "#011083", "#130183", "#330183", "#540183", "#011083",
           "#330183", "#740183", "#830151", "#830110" ,"#0c3205","#2b0532",
           "#32050c","#0c7867","#0A6028","#17600A","#0F8E3B","#0A6053",
           "#0A4053","#0A4260","#0A1760","#0F628E", "#42600A")

# Add  more colours if you need

```

This is just a vector with all your colours. Add as many colours as you like, but the number of colours must exceede the number of colours needed for your plot. ggplot will pick the colours in the order that you put them, so keep your favorite colours at the start. You could also add multiple vectors with colour if this is needed. Visit this site to find colours that go well toghether  
https://htmlcolorcodes.com/color-picker/

Next create a new file containing the actual theme  
> File - New File - R Script  
and call it **custom_theme.R** 

```
.pkgenv <- new.env(parent=emptyenv())

.pkgenv[["theme_custom_base"]] <- function(legend=0){
  theme_minimal() %+replace%
    theme(
      #change font
      text=element_text(family="gochi"), # need to include zzz.R

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
This will govern the actual apperance of your theme. To learn how to change the different aspects of your ggplot refere to this page:  
https://ggplot2.tidyverse.org/reference/theme.html  

The first line in this script, the **.pkgenv <- new.env(parent=emptyenv())** make sure that these functions are only available inside the library. This is not strictly neccisary, but it help to keep your R enviroment clean. 

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

Notice the **lengend** and **palette** argument that is called here. This is the same argument that we use in **custom_theme** and we use this to make changes to the theme from the actual function call in Rstudio.  

At last we add the file zzz.R  

> File - New File - R Script  
and call it **zzz.R** 

The final step will add your custom fonts. This is done through the package showtext and sysfonts.

```
.onLoad <- function(libname, pkgname) {
  sysfonts::font_add_google("Gochi Hand", "gochi")
  showtext::showtext_auto()
}
```
The R folder should now look like this:  
![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/a8580b5e-19f6-43b4-8dfe-2aef86accb7b)

# Add files to man

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
Notice how I use the legend arguemnt to tilt the legend 25 degrees in the exemple. This can be done since we implemented it previously  

your man folder should now look like this:  
![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/5650c7ec-e960-4115-aa22-e7970a4b9542)

# Edit the description and namespace file

Open the description file, that is located in your project directory, and make sure to change the "collate" and "imports" to the following. This is an important step. The collate argument tell R in what order the different R files should be run. Since we have some dependencies in our files it is important that we run them in this order. The import argument tell R what libraries that this package is dependent on. The two first "curl" and "ggplot2" is only there to allow the expamples in the man files run, and could be droped if you dont show any exemples in the man files.

Sysfont and showtext is there to allow import of fonts.

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/84df9610-be78-431f-be12-e1d2aceb942b)


```
Package: customTheme
Type: Package
Title: Controll font and colour of plots 
Version: 0.1.0
Author: yourName
Maintainer: The package maintainer <your@e.mail>
Description: More about what it does (maybe more than one line)
    Use four spaces when indenting paragraphs within the Description.
License: MIT License + file LICENSE
Encoding: UTF-8
LazyData: true
Collate:
  custom_colour.R
  custom_theme.R
  mainFunction.R
  zzz.R
Imports: 
    curl,
    ggplot2,
    jsonlite,
    sysfonts,
    showtext
```

The namespace file should look like this:

```
exportPattern("^[[:alpha:]]+")

```

# Installing your package
Next step would be to installl your package. You do this in the **build** tab in rStudio and the response should look something like this:  

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/dc42f712-4951-4db0-b150-fdb42d9f911d)

Now your package should be ready to be used and you could test it by running this code in a new R script:

```
library(customTheme)
library(tidyverse)

starwars %>%
  head(23) %>%
  ggplot()+
  geom_col(aes(homeworld,height, fill=skin_color), position = "dodge")+
  labs(title = "Age in starwars", subtitle = "for different planets and eye colour", caption = "DataSource : Starwars")+
  theme_custom(legend = 25)

```

# push the package to github

to allow others to also use your packgage I recomend uploading it to git, this way others could easly get the package to run in their own R sessions.

first load devtools again

> library(devtools)

Then run **use_git**
> use_git()

The response would look like this:  

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/f66742ce-b6c8-4cda-8de6-f6391f6e3785)


Answare "Yup" and get the next response 

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/bf30b57a-a3a4-4a2b-aa97-68ebc76974e0)


Rstudio will now restart and you will see a **Git** pane next to **Build**

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/ce4763c3-fd88-4284-bf23-ad0371594f1b)

Finally we run the function use_github()  

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/319ff4ba-6c7c-4b0e-9124-c7998537715c)

now anyone can install your theme with the following command: 


```
library(devtools)
install_github("your_username/customtheme")

and add theme_custom() to their ggplot() like this

ggplot()
 geom_col(aes(x,y)+
 theme_custom()

```


# change fonts

the fonts for the package have been taken from https://fonts.google.com/
If you want to change the font of your plot you must change it both in ***zzz.R*** and in **custom_theme.R** !!

```
in zzz.R  
sysfonts::font_add_google("Gochi Hand", "gochi") # change "Gochi Hand" to something else to load a different font

in cusomTheme.R  
text=element_text(family="gochi"), # need to include zzz.R

```

# make changes to your package and push to git  

To make changes to the package, simply make the changes in the R files and push to git with the Git pane in RStudio
whenever you make save a change to any files associated with your package it will show up in the git-pane.  
Click "staged" and push commit. Why must I click "staged" you may ask : [githowto](https://githowto.com/staging_and_committing)

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/1168c35f-c5f6-4986-8dfa-51b0f9805790)

It will show a new window and you can add some descrition to the change and press "commit"

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/2ce845b1-24c3-4693-a671-6e054ec310b0)






