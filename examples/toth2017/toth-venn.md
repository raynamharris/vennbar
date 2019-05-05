Cognitive specialization for learning faces is associated with shifts in the brain transcriptome of a social wasp
=================================================================================================================

This venn diagram is from [this
paper](http://jeb.biologists.org/content/220/12/2149)

![](GOvenn-original.png)

    library(ggplot2)
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

    p2 <- ggdraw() + draw_image("GOvenn-original.png", scale = 0.9)
    plot_grid(p2, p, rel_widths = c(0.4,0.6),
              labels = c("venn", "bar"), label_size = 8)

![](./toth-original-alt-1.png)