Early life stress alters transcriptomic patterning across reward circuitry in male and female mice
==================================================================================================

This venn diagram is from [this
paper](https://www.biorxiv.org/content/10.1101/624353v1)

![](Pena2019.png)

    library(ggplot2)
    library(dplyr)
    library(cowplot)

    knitr::opts_chunk$set(fig.path = './', echo = T, message = F)

    # colors from imagecolorpicker.com
    fig2venn <- read.csv("fig2venn.csv")

    #head(fig2venn)

    #summary(fig2venn)

    fig2venn$region <- factor(fig2venn$region, levels = c("VTA", "NAc", "PFC")) 
    fig2venn$group <- factor(fig2venn$group, levels = c("All" ,  
                                                         "Defeat/STVS and ELS + Defeat/STVS",
                                                         "ELS and ELS + Defeat/STVS" ,
                                                         "ELS and Defeat/STVS",
                                                         "ELS + Defeat/STVS",
                                                         "Defeat/STVS",
                                                         "ELS" 
                                                         ))


    males <- fig2venn %>% filter(sex == "male") %>% droplevels()
    females <- fig2venn %>% filter(sex == "female") %>% droplevels()

    malecolors <- c("All" = "#79BDDE",  "Defeat/STVS and ELS + Defeat/STVS" = "#A7C8D9", "ELS and ELS + Defeat/STVS" = "#03BEEE" ,
                    "ELS and Defeat/STVS" = "#8DD6F2", "ELS + Defeat/STVS" = "#92C9DE", "Defeat/STVS" = "#DDDEE0", "ELS" = "#69D7FC" )

    femalecolors <- c("All" = "#DE87B3", "Defeat/STVS and ELS + Defeat/STVS" = "#D4AEC9", "ELS and ELS + Defeat/STVS" = "#FF469F" ,
                     "ELS and Defeat/STVS" = "#E2A1BF", "ELS + Defeat/STVS" = "#F3A4D5", "Defeat/STVS" = "#DDDEE0", "ELS" = "#FF80BB" )

    sharedunshared <- c("All" = "#810f7c",  "Defeat/STVS and ELS + Defeat/STVS" = "#8c6bb1", "ELS and ELS + Defeat/STVS" = "#8c96c6" ,
                    "ELS and Defeat/STVS" = "#9ebcda", "ELS + Defeat/STVS" = "#fdb863", "Defeat/STVS" = "#e08214", "ELS" = "#b35806" )


    m <- ggplot(data=males, aes(x=group, y = number,  fill = group)) + 
      geom_bar(stat="identity")  + 
      coord_flip() + 
      facet_wrap(~region, nrow = 3) +
      scale_x_discrete(drop=FALSE)+
      scale_y_continuous(limits=c(0, 325)) +
      scale_fill_manual(values = malecolors) +
      geom_text(aes(label=number),size=3, hjust = -0.05) +
      guides(fill = guide_legend(reverse = TRUE)) +
      theme(legend.title = element_blank(),
            legend.position = "none",
            strip.background = element_rect( fill="white")) +
      labs(subtitle = "Male response",
           x = NULL, y = "number of DEGs") 
    #m

    f <- ggplot(data=females, aes(x=group, y = number,  fill = group)) + 
      geom_bar(stat="identity")  + 
      coord_flip() + 
      facet_wrap(~region, nrow = 3) +
      scale_x_discrete(drop=FALSE)+
      scale_y_continuous(limits=c(0, 325)) +
      scale_fill_manual(values = femalecolors) +
      geom_text(aes(label=number),size=3, hjust = -0.05) +
      guides(fill = guide_legend(reverse = TRUE)) +
      theme(legend.title = element_blank(),
            legend.position = "none",
            strip.background = element_rect( fill="white")) +
      labs(subtitle = "Female response",
           x = NULL, y = "number of DEGs") 
    #f

    plot_grid(m, f + theme(axis.text.y=element_blank()), 
              nrow = 1, rel_widths = c(0.7,0.3))

![](./fig2venn-alt1-1.png)

    fig2venn$percent <- (fig2venn$number / sum(fig2venn$number) * 100)

    ggplot(fig2venn, aes(x = region, y = percent, fill = reorder(group, desc(group)))) + 
      geom_bar(stat = "identity") + 
      theme(legend.title = element_blank(),
            legend.position = "bottom") + 
      labs(x = NULL, y = "% DEGs",
           caption = "One alternative with % instead of counts.") +
      facet_wrap(~sex) +
      scale_fill_manual(values = sharedunshared) 

![](./fig2venn-alt2-1.png)

    subsetpercent <- function(whichsex, whichregion){
      newdf <- fig2venn %>% filter(sex == whichsex, region == whichregion)%>% droplevels()
      newdf$percent <- (newdf$number / sum(newdf$number) * 100)
      return(newdf)
    }

    MVTA <- subsetpercent("male", "VTA")
    MNAc <- subsetpercent("male", "NAc")
    MPFC <- subsetpercent("male", "PFC")
    FVTA <- subsetpercent("female", "VTA")
    FNAc <- subsetpercent("female", "NAc")
    FPFC <- subsetpercent("female", "PFC")

    fig2venn <- rbind(MVTA, MNAc,MPFC,FVTA, FNAc, FPFC)


    #fig2venn$sex <- factor(fig2venn$sex, levels = c("female", "male"))

    ggplot(fig2venn, aes(x = region, y = percent, fill = reorder(group, desc(group)))) + 
      geom_bar(stat = "identity") + 
      #theme_bw(base_size = 9) +
      theme(legend.title = element_blank(),
            legend.position = "top",
            legend.text = element_text(size = 7),
            legend.key.size = unit(0.25, "cm"))  + 
      labs(x = NULL, y = "% DEGs") +
      #geom_hline(yintercept=22, linetype="dashed") +
      #geom_hline(yintercept=50, linetype="dashed") +
      facet_wrap(~sex) +
      scale_fill_manual(values = sharedunshared) +
      guides(fill = guide_legend(nrow = 3)) 

![](./fig2venn-alt2-2.png)

    fig2venn$region <- factor(fig2venn$region, levels = c("PFC", "NAc", "VTA"))

    p2 <- ggplot(fig2venn, aes(x = region, y = percent, fill = group)) + 
      geom_bar(stat = "identity") + 
      #theme_bw(base_size = 9) +  
      theme_minimal() +
      theme(legend.title = element_blank(),
            legend.position = "bottom",
            legend.text = element_text(size = 7),
            legend.key.size = unit(0.25, "cm"))  + 
      labs(x = NULL, y = "% DEGs") +
      #geom_hline(yintercept=22, linetype="dashed") +
      #geom_hline(yintercept=50, linetype="dashed") +
      facet_wrap(~sex) +
      scale_fill_manual(values = sharedunshared) +
      guides(fill = guide_legend(nrow = 4)) +
      coord_flip()  +
      guides(fill = guide_legend(reverse = TRUE))
    p2

![](./fig2venn-alt2-3.png)

    p1 <- ggdraw() + draw_image("fig2venn-original.png", scale = 0.9)
    plot_grid(p1, p2,  rel_widths = c(0.45,0.55))

![](./pena-original-alt-1.png)

Here, the Venn diagrams are also scales, but each of the six subplots
has their own scaling, so you cannot compare all six visually. Moreover,
the authors report the *percent* of shared gene expression in the
manuscript, but their Venn diagrams show *counts*. So, I calculated the
percent of shared differentially expressed genes (%DEGs) and colored all
overlapping responses in shades of purple and the unique responses in
shades of orange. Now, once can spot interesting trends in the data,
like increase response to early and late life stress (ELS + STVS) in the
female PFC. This is something that is present but not emphasized in the
Venn.
