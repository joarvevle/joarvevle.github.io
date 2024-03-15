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

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/2ac79d10-dda2-434a-a6f4-dbc5567c2e29)

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/2df4b7a6-f3ee-4412-ab78-74a882d7ba6e)

This will create a new folder names customTheme with a number of files.   

![image](https://github.com/joarvevle/joarvevle.github.io/assets/143795683/b01c17a9-d94a-4181-8b82-8c4a5190d7dc)

We must now add some R files to the package. Your R files should go to the R folder:  

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

custom_old =  c("#05322b","#0a6053","#018374","#02BC94","#949494","#595959",
           "#006f79", "#00a6b8","#015183", "#fcb614","#da8220","#837401",
           "#939393","#D0D0D0","#143C36", "#244540", "#334F4B", "#425955",
           "#526260", "#616C6A", "#707675", "#244C46", "#446560", "#637F7A",
           "#013183", "#011083", "#130183", "#330183", "#540183", "#011083",
           "#330183", "#740183", "#830151", "#830110" ,"#0c3205","#2b0532",
           "#32050c","#0c7867","#0A6028","#17600A","#0F8E3B","#0A6053",
           "#0A4053","#0A4260","#0A1760","#0F628E", "#42600A")


custom_light = c("#018374","#02BC94","#949494","#595959","#006f79", "#00a6b8"
             ,"#015183", "#fcb614","#da8220","#837401","#939393","#D0D0D0"
             ,"#017366","#016257","#015248","#219385","#0c7867","#41A297",
             "#60B2A8","#80C1BA" ,"#A0D1CB","#C0E0DC","#118375","#308278",
             "#50817B", "#60807D", "#70807E","#0A6028","#17600A","#0F8E3B",
             "#0A6053","#0A4053","#0A4260","#0A1760","#0F628E","#0c5205",
             "#1b2232","#39350c",  "#244C46", "#446560", "#637F7A")




#Add two colour schemes for use with dark mode graph

.pkgenv[["custom_fill_light"]] <- function(){
  scale_fill_manual(values = custom_light )
}

.pkgenv[["custom_colour_light"]] <- function(){
  scale_colour_manual(values = custom_light )
}

#Add two colour schemes for use with dark mode graph

.pkgenv[["custom_fill_old"]] <- function(){
  scale_fill_manual(values = custom_old )
}

.pkgenv[["custom_colour_old"]] <- function(){
  scale_colour_manual(values = custom_old )
}
```


