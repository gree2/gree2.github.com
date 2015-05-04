---
layout: post
title: "python seaborn tutorial 1 example gallery"
description: ""
category: [python]
tags: [seaborn, tutorial]
---
{% include JB/setup %}


### example gallery

1. [implot - anscombe's quartet](http://stanford.edu/~mwaskom/software/seaborn/examples/anscombes_quartet.html)

1. [barplot - color palette choices](http://stanford.edu/~mwaskom/software/seaborn/examples/color_palettes.html)

1. [kdeplot](http://stanford.edu/~mwaskom/software/seaborn/examples/cubehelix_palette.html)

1. [distplot](http://stanford.edu/~mwaskom/software/seaborn/examples/distplot_options.html)

1. [violinplot](http://stanford.edu/~mwaskom/software/seaborn/examples/elaborate_violinplot.html)

1. [FacedGrid](http://stanford.edu/~mwaskom/software/seaborn/examples/faceted_histogram.html)

1. [FacedGrid](http://stanford.edu/~mwaskom/software/seaborn/examples/facets_with_custom_projection.html)

1. [factorplot](http://stanford.edu/~mwaskom/software/seaborn/examples/factorplot_bars.html)

1. [factorplot](http://stanford.edu/~mwaskom/software/seaborn/examples/grouped_boxplot.html)

1. [heatmap](http://stanford.edu/~mwaskom/software/seaborn/examples/heatmap_annotation.html)

1. [jointplot](http://stanford.edu/~mwaskom/software/seaborn/examples/hexbin_marginals.html)

1. [interactplot](http://stanford.edu/~mwaskom/software/seaborn/examples/interactplot.html)

1. [jointplot](http://stanford.edu/~mwaskom/software/seaborn/examples/joint_kde.html)

1. [Implot](http://stanford.edu/~mwaskom/software/seaborn/examples/logistic_regression.html)

1. [FacedGrid](http://stanford.edu/~mwaskom/software/seaborn/examples/many_facets.html)

1. [corrplot](http://stanford.edu/~mwaskom/software/seaborn/examples/many_pairwise_correlations.html)

1. [JointPlot](http://stanford.edu/~mwaskom/software/seaborn/examples/marginal_ticks.html)

1. [Implot](http://stanford.edu/~mwaskom/software/seaborn/examples/multiple_regression.html)

1. [heatmap](http://stanford.edu/~mwaskom/software/seaborn/examples/network_correlations.html)

1. [PairGrid](http://stanford.edu/~mwaskom/software/seaborn/examples/pair_grid_with_kde.html)

1. [PairGrid](http://stanford.edu/~mwaskom/software/seaborn/examples/paired_pointplots.html)

1. [factorplot](http://stanford.edu/~mwaskom/software/seaborn/examples/pointplot_anova.html)

1. [jointplot](http://stanford.edu/~mwaskom/software/seaborn/examples/regression_marginals.html)

1. [residplot](http://stanford.edu/~mwaskom/software/seaborn/examples/residplot.html)

1. [pairplot](http://stanford.edu/~mwaskom/software/seaborn/examples/scatterplot_matrix.html)

1. [violinplot](http://stanford.edu/~mwaskom/software/seaborn/examples/simple_violinplots.html)

1. [clustermap](http://stanford.edu/~mwaskom/software/seaborn/examples/structured_heatmap.html)

1. [tsplot](http://stanford.edu/~mwaskom/software/seaborn/examples/timeseries_bootstrapped.html)

1. [tsplot](http://stanford.edu/~mwaskom/software/seaborn/examples/timeseries_from_dataframe.html)

1. [factorplot](http://stanford.edu/~mwaskom/software/seaborn/examples/timeseries_of_barplots.html)

### import

            import seaborn as sns
            %matplotlib inline
            import numpy as np
            import matplotlib.pyplot as plt

### 1. [implot - anscombe's quartet](http://stanford.edu/~mwaskom/software/seaborn/examples/anscombes_quartet.html)

            sns.set(style='ticks')
            df = sns.load_dataset('anscombe')
            sns.lmplot('x', 'y', col='dataset', hue='dataset',
                       data=df, col_wrap=2, ci=None, palette='muted',
                       size=4, scatter_kws={'s': 50, 'alpha': 1})

### 2. [barplot - color palette choices](http://stanford.edu/~mwaskom/software/seaborn/examples/color_palettes.html)

            sns.set(style='white', context='talk')
            rs = np.random.RandomState(7)

            x = np.array(list('ABCDEFGHI'))

            f, (ax1, ax2, ax3) = plt.subplots(3, 1, figsize=(8, 6), sharex=True)

            y1 = np.arange(1, 10)
            # array([1, 2, 3, 4, 5, 6, 7, 8, 9])
            sns.barplot(x, y1, ci=None, palette='BuGn_d', hline=1, ax=ax1)
            ax1.set_ylabel('Sequential')

            y2 = y1 - 5
            # array([-4, -3, -2, -1,  0,  1,  2,  3,  4])
            sns.barplot(x, y2, ci=None, palette='coolwarm', hline=0, ax=ax2)
            ax2.set_ylabel('Diverging')

            y3 = rs.choice(y1, 9, replace=False)
            # array([3, 8, 1, 9, 6, 4, 7, 2, 5])
            sns.barplot(x, y3, ci=None, palette='Paired', hline=.1, ax=ax3)
            ax3.set_ylabel('Qualitative')

            sns.despine(bottom=True)
            plt.setp(f.axes, yticks=[])
            plt.tight_layout(h_pad=3)