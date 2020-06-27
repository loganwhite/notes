## The following code snippet demonstrates the plot in plot style


```python3
def plot_fct():
    
    data = read_file()

    x = [1,2,4,6,8,10]
    y = [] # flow size
    fcts = []
    # for size_data in data:
    #     # paths
    size_data = data[-1]
    for path_data in size_data:
        d = np.transpose(path_data)
        fct = d[1]
        #print(fct)
        fcts.append(fct)
        y.append(sum(fct)/len(fct))

    # start preparing the plot
    plt.close('all')
    plt.figure()

    #plt.ylim(0.7, 10000000)

    # for i in range(len(y)):
    #     plt.plot(x, y[i], marker=marker[i])
    plt.plot(x, y, label=flowsize[-1])
    plt.boxplot(fcts, positions=x)
    ax = plt.gca()

    small_tick_font = 14

    ins_1 = ax.inset_axes([0.1,0.75,0.1,0.2])
    ins_1.tick_params(labelsize=small_tick_font)
    ins_1.yaxis.tick_right()
    ins_1.boxplot([fcts[0]])
    ins_1.set_xticks([])

    ins_2 = ax.inset_axes([0.02,0.2,0.1,0.2])
    ins_2.tick_params(labelsize=small_tick_font)
    ins_2.yaxis.tick_right()
    ins_2.boxplot([fcts[1]])
    ins_2.set_xticks([])

    ins_3 = ax.inset_axes([0.25,0.27,0.1,0.2])
    ins_3.tick_params(labelsize=small_tick_font)
    ins_3.yaxis.tick_right()
    ins_3.ticklabel_format(useOffset=False)
    ins_3.boxplot([fcts[2]])
    ins_3.set_xticks([])


    ins_4 = ax.inset_axes([0.5,0.15,0.1,0.2])
    ins_4.tick_params(labelsize=small_tick_font)
    # ins_4.yaxis.tick_right()
    ins_4.boxplot([fcts[3]])
    ins_4.set_xticks([])

    ins_5 = ax.inset_axes([0.7,0.1,0.1,0.2])
    ins_5.tick_params(labelsize=small_tick_font)
    ins_5.boxplot([fcts[4]])
    ins_5.set_xticks([])

    ins_6 = ax.inset_axes([0.9,0.06,0.1,0.2])
    ins_6.tick_params(labelsize=small_tick_font)
    ins_6.boxplot([fcts[5]])
    ins_6.set_xticks([])
    


    plt.xlabel('Number of Paths', fontsize=25)
    plt.ylabel('FCT (sec)',fontsize=25)
    plt.grid(True)


    xlabels = ['1','2','4','6','8','10']
    plt.xticks(x,xlabels)
    # plt.ylim(ymin=0.005,ymax=20000)
    
    # scale the x axis to log and make it good 
    # looking (show in the scientific expression way)
    # plt.yscale('log')

    plt.legend()
    plt.tight_layout(True)
    
    # save to pdf file
    filename = "fct"
    plt.savefig('{}.pdf'.format(filename), format='pdf', dpi=900)

    plt.show()
    

plot_fct()
```
