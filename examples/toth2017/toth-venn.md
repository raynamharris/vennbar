Cognitive specialization for learning faces is associated with shifts in the brain transcriptome of a social wasp
=================================================================================================================

This venn diagram is from [this
paper](http://jeb.biologists.org/content/220/12/2149)

![](GOvenn-original.png)

    library(tidyverse)
    library(cowplot)

    knitr::opts_chunk$set(fig.path = './', echo = T, message = F)

    GOvenn <- read.csv(file = "./GOvenn.csv")
    #head(GOvenn)

    GOvenn$species <- factor(GOvenn$species, levels = c("P.fusatus", "P.metricus",  "both"))

    levels(GOvenn$GO) <- c("biological processes", " molecular functions")

    p <- ggplot(data=GOvenn, aes(x=pattern, y = count,  fill = species)) + 
      geom_bar(stat="identity")  + 
      coord_flip() +
      facet_wrap(~GO, nrow = 2) +
      geom_text(position = "stack", aes(x=pattern, y = count,  label = count, hjust = 0.5)) +
      theme(legend.position = "top",
            legend.title = element_blank()) +
      labs(x = NULL, y = "total GO terms")
    p

![](./GOvenn-alt-1.png)

    p2 <- ggdraw() + draw_image("GOvenn-original.png")
    plot_grid(p2, p, rel_widths = c(0.5,0.5),
              labels = c("venn", "bar"), label_size = 8)

![](./toth-original-alt-1.png)

    GOvenn %>%
      spread(species, count) %>%
      select(GO, pattern,P.fusatus, both, P.metricus)

    ##                     GO  pattern P.fusatus both P.metricus
    ## 1 biological processes expected        52    0         30
    ## 2 biological processes observed        42   10         20
    ## 3  molecular functions expected        43    1         37
    ## 4  molecular functions observed        31   13         25
