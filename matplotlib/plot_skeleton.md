## This file is a rather clean skeleton that can be used.

```python3
#! /usr/local/bin/python3

import numpy as np
import matplotlib.pyplot as plt
# This import registers the 3D projection, but is otherwise unused.
from matplotlib import ticker, cm, rc
import matplotlib
import os
import matplotlib.pylab as pylab
from matplotlib.ticker import FuncFormatter, MultipleLocator, FormatStrFormatter, ScalarFormatter

# import data file
from profile_i_d_data import *
from raw_data import *

# import util functions
from util import *

# three type of ways to configure the global parameters
rc('text', usetex=True)
matplotlib.rcParams['text.latex.unicode']=True
matplotlib.rcParams['font.family'] = ['serif']
matplotlib.rcParams['font.serif'] = ['times']
# beautiful font
matplotlib.rcParams['pdf.use14corefonts'] = True

params = {
    'axes.labelsize' : '12',
    'xtick.labelsize' : '12',
    'ytick.labelsize' : '12',
    'lines.linewidth' : '0.5',
    'legend.fontsize' : '12',
    'legend.title_fontsize' : '12',
    'figure.figsize' : '5, 3.5',
}

# marker and linestyle libs
marker = ['o', 'v', '^', '<', '>', '8', 's', 'p', 'd', 
            '*', 'h', 'H', 'D', 'X', 'P']
linestyle = ['--', '-', ':', '-.', '-','-','-']

# beautiful color patterns
COLORS = ['#1f77b4', '#ff7f0e', '#2ca02c', '#d62728',
          '#9467bd', '#8c564b', '#e377c2', '#7f7f7f',
          '#bcbd22', '#17becf']


pylab.rcParams.update(params)


def plot_statesize_perf():
    # data from raw data.py
    data = [pf_nm20_rand_TempLocality_0res_nf_mpps, pf_nm20_rand_TempLocality_2res_nf_mpps,
    pf_nm20_rand_TempLocality_4res_nf_mpps, pf_nm20_rand_TempLocality_8res_nf_mpps,
    pf_nm20_rand_TempLocality_16res_nf_mpps, pf_nm20_rand_TempLocality_32res_nf_mpps]

    x = np.asarray(state_size)
    y = []
    legend = [0.0,2,4,8,16,32]
    # transform legend
    # legend = np.divide(legend, max(legend)).tolist()
    legend = np.divide(legend, max(legend)).tolist()


    #plot the number of temloc lines
    for i in range(len(data)):
        y.append(data[i]['64'])
    # convert y to numpy object in case of further possible issues
    y = np.asarray(y)

    # start preparing the plot
    plt.close('all')
    plt.figure()

    for i in range(len(y)):
        plt.plot(x, y[i], marker=marker[i])
    plt.xlabel('State Size (\# of entries)')
    plt.ylabel('Throuput (Mpps)')
    
    # scale the x axis to log and make it good 
    # looking (show in the scientific expression way)
    plt.xscale('log')
    
    # draw a vertical line
    plt.plt.axvline(x = 45000, linewidth=0.8, ls='dashed')
    
    # get current axe
    ax = plt.gca()
    ax.set_xlim([400, 2000000])
    min_mpps = 60
    max_mpps = 140
    ax.set_ylim([min_mpps, max_mpps])
    
    # annotate with arrow
    ax.annotate('This is a \\\\turning point', xy=(60000, 130), xytext=(500000, 125),
            # note that the arrow color is changed with the color attribute.
            arrowprops=dict(arrowstyle="->", facecolor='black', color=COLORS[0]))

    plt.legend(legend, title='Temporal locality')
    plt.tight_layout(True)
    
    # save to pdf file
    filename = "statesize_mpps_NM"
    plt.savefig('{}.pdf'.format(filename), format='pdf', dpi=900)

    plt.show()
    

plot_statesize_perf()
```
