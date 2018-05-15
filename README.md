# Simple event generator
Not so long ago I was searching for event generator that allows to generate a couple of thousand events like sensor data. I looked for a bit of code or a small application but all I found were hardware event generators for automobile and a huge generator here on github. So I decided to write it by myself. Althogh it's far from perfect, there are some plans for improvement but I intend to keep it as simple as possible.

## Description
A quite simple event generator which allows to simulate sensors and create quite large amount of points (up to 10 millon, but depends on memory)
This is, in fact, a function written in Python which creates numbers one by one. At the moment it's possible to customize how exactly this is done, but there are not so much customization options yet. 

For now it's implementation supposes that there are small sub-periods where the values are equal. At the end of each sub-period function makes a desicion in which direction the series must be changed (stay still, go down or go up). This decision is based on decision on previous step, e.g. it's more likely that series will go down if previous step also went down.
Function requires *numpy* and *scipy.stats* to be imported.
Currently it's implemented as Jupyter notebook

## Inputs:
**period_len** - a number of events to generate. It's recommended to use values from 10000 to 10000000. If less - try to set **length_random** as *False* and set a **length** to a lesser values (e.g. up to 100). If you set more than 10 million, it may cause a memory overflow

**length** - length of a single sub-period where function outputs are equal. 

**default_dir**(default == 'Still') - the direction of a first change. For now it's not very useful because thre are many decisions of directions. This parameter most likely will be deprecated if decision-making logic will not change dramatically. Values: 'Still', 'Down' and 'Up'.

**still_prob**(optional, default == 0.5) - a probability of 'Still' direction choice for each next step. This is an attempt to provide a variety to series. This is implemented because each step direction depends on previous direction, so this parameter tries to reset previous trend and give series a chance to statr a new one.

**deviation**(default == 1) - used to calculate _step_ as choice from *np.linspace(0, dev * 2, 100)*, based on normal distibution, so step is normally distributed around **deviation** divided **length**, and remains unchanged during all the process of series generating

**length_random** (optional, default == _False_) - flag of randomizing _step_. If _True_,  _step_ is calculated as _np.random.randint(period_len/10000, period_len/1000)_. NOTE: this is imperfect for generating short series (lesser than 10000 points), but works for long ones

**direction_random** (optional, default == _False_) - flag of randomizing _direction_. If  == _True_,  **default_dir** is ignored and first direction has equal chances for all thee options


# Output
Numpy array of shape = (**period_len**,)

# Plans:
1) create more complicated (and adjustable) strategy for choosing next steps
2) (maybe) reconsider the logic of step calculation
3) add an option to directly set deviation (now it's stochastic from 0 to **deviation * 2**)
4) add an option to choose a different distributions for _step_ calculation
5) add some features to generate series for specific cases
6) find a way to optimize fuction to generate large amounts os points
