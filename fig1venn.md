    library(ggplot2)
    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    library(cowplot)

    ## 
    ## Attaching package: 'cowplot'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     ggsave

    knitr::opts_chunk$set(fig.path = './')

    fig1venn <- read.csv("fig1venn.csv")
    summary(fig1venn)

    ##  panel     sex     region                     group       number     
    ##  a:7   female:21   NAc:14   All                  :6   Min.   :  6.0  
    ##  f:7   male  :21   PFC:14   ELS                  :6   1st Qu.: 14.5  
    ##  i:7               VTA:14   ELS & ELS + Stress   :6   Median : 51.0  
    ##  n:7                        ELS & Stress         :6   Mean   : 83.4  
    ##  q:7                        ELS + Stress         :6   3rd Qu.:132.2  
    ##  v:7                        Stress               :6   Max.   :307.0  
    ##                             Stress & ELS + Stress:6

    levels(fig1venn$group)

    ## [1] "All"                   "ELS"                   "ELS & ELS + Stress"   
    ## [4] "ELS & Stress"          "ELS + Stress"          "Stress"               
    ## [7] "Stress & ELS + Stress"

    fig1venn$region <- factor(fig1venn$region, levels = c("VTA", "NAc", "PFC")) 
    fig1venn$group <- factor(fig1venn$group, levels = c("All" ,  
                                                           "Stress & ELS + Stress",
                                                           "ELS & ELS + Stress" ,
                                                           "ELS & Stress",
                                                           "ELS + Stress",
                                                           "Stress",
                                                           "ELS" 
                                                           ))


    summary(fig1venn)

    ##  panel     sex     region                     group       number     
    ##  a:7   female:21   VTA:14   All                  :6   Min.   :  6.0  
    ##  f:7   male  :21   NAc:14   Stress & ELS + Stress:6   1st Qu.: 14.5  
    ##  i:7               PFC:14   ELS & ELS + Stress   :6   Median : 51.0  
    ##  n:7                        ELS & Stress         :6   Mean   : 83.4  
    ##  q:7                        ELS + Stress         :6   3rd Qu.:132.2  
    ##  v:7                        Stress               :6   Max.   :307.0  
    ##                             ELS                  :6

    males <- fig1venn %>% filter(sex == "male") %>% droplevels()
    females <- fig1venn %>% filter(sex == "female") %>% droplevels()


    m <- ggplot(data=males, aes(x=group, y = number,  fill = group)) + 
      geom_bar(stat="identity")  + 
      coord_flip() + 
      facet_wrap(~region, nrow = 3) +
      scale_x_discrete(drop=FALSE)+
      scale_y_continuous(limits=c(0, 325)) +
      scale_fill_manual(values = c("All" = "#79BDDE",  
                                    "Stress & ELS + Stress" = "#A7C8D9",
                                    "ELS & ELS + Stress" = "#03BEEE" ,
                                    "ELS & Stress" = "#8DD6F2",
                                    "ELS + Stress" = "#92C9DE",
                                    "Stress" = "#DDDEE0",
                                    "ELS" = "#69D7FC" )) +
      geom_text(aes(label=number),size=3, hjust = -0.05) +
      guides(fill = guide_legend(reverse = TRUE)) +
      theme(legend.title = element_blank(),
            legend.position = "none") +
      labs(subtitle = "Male response to stress",
           x = NULL, y = "number of DEGs") 


    f <- ggplot(data=females, aes(x=group, y = number,  fill = group)) + 
      geom_bar(stat="identity")  + 
      coord_flip() + 
      facet_wrap(~region, nrow = 3) +
      scale_x_discrete(drop=FALSE)+
      scale_y_continuous(limits=c(0, 325)) +
      scale_fill_manual(values = c("All" = "#DE87B3",  
                                    "Stress & ELS + Stress" = "#D4AEC9",
                                    "ELS & ELS + Stress" = "#FF469F" ,
                                    "ELS & Stress" = "#E2A1BF",
                                    "ELS + Stress" = "#F3A4D5",
                                    "Stress" = "#DDDEE0",
                                    "ELS" = "#FF80BB" )) +
      geom_text(aes(label=number),size=3, hjust = -0.05) +
      guides(fill = guide_legend(reverse = TRUE)) +
      theme(legend.title = element_blank(),
            legend.position = "none",
            axis.text.y=element_blank()) +
      labs(subtitle = "Female response to stress",
           x = NULL, y = "number of DEGs") 


    plot_grid(m,f, nrow = 1, rel_widths = c(0.6,0.4))

![](./fig1venn-1.png)
