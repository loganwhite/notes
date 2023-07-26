Calculating CDF using the following code snnipet:

```python3
def plot_cdf(data):
    # start preparing the plot
    plt.close('all')
    plt.figure()

    step=0.01
    indices = np.arange(0, 1 + step, step)

    CDF = []

    for data_list in data:
        CDF.append(pd.DataFrame({'dummy':data_list})['dummy'].quantile(indices))

    for i in range(len(CDF)):
        plt.plot(CDF[i], indices, linewidth=1, label=data_labels[i], color=COLORS[i%len(COLORS)])

    plt.xlabel('Counter Values')
    plt.ylabel('CDF')
    
    # plt.xscale('log')
  
    plt.legend(ncol=6, bbox_to_anchor=(-0.14, 1), title='$\\omega$ Values', loc='lower left', columnspacing=0.1)
    # plt.legend(loc='lower left', ncol=5, bbox_to_anchor=(-0.09, 1))
    plt.tight_layout()
    

    # save to pdf file
    
    plt.savefig('{}.pdf'.format(filename), format='pdf', dpi=900)
```
