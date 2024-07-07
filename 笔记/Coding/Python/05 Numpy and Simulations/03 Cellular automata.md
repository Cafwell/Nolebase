
## Animation of fire model
```Python
import random
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
from matplotlib.colors import ListedColormap
from matplotlib.widgets import Button

# probability of a tree igniting (f), probability of an empty cell becoming a tree (p) and the probability of rain (r_p)
f = 0.01
p = 0.06
rain_prob = 0.02

# build a square background
cols = 50
rows = 50
print(f'The grid size is {cols}×{rows}!')
print(f'The probability of a tree igniting (f) is {f}; of an empty cell becoming a tree (p) is {p}; of rain is {rain_prob}.')

# create a grid of cells, with each cell initially set to empty
grid = np.zeros((cols, rows))
  
# set the start state of the grid, 70% are trees
random.seed(2023)
for i in range(rows):
    for j in range(cols):
        if random.random() < 0.7:  # random.random() used to randomly generates floating-point numbers between (0,1)
            grid[i, j] = 1
        else:
            grid[i, j] = 0

# defines state constants
Empty = 0
Tree = 1
Burning = 2
Rain = 3
Lake = 4

# generate a random lake region
max_lake = round(0.2 * rows)
np.random.seed(2023)
lake_row = np.random.randint(rows)
lake_col = np.random.randint(cols)
lake_size = np.random.randint(0, max_lake)  # side length of the random lake will between 0 and 'max_lake' cells

# Flag to indicate whether the simulation is paused
paused = False
# Flag to indicate whether a manual fire has been triggered
manual_fire = False
# set a counter
counter = 0
# count the cells
tree_percent = []
fire_percent = []

def update(n, rain_prob):
    global grid, paused, manual_fire, counter
    if not paused:
        new_grid = np.copy(grid)
        for i in range(rows):
            for j in range(cols):
                # check if the cell is within the lake region
                in_lake = lake_row <= i < lake_row + lake_size and lake_col <= j < lake_col + lake_size
                if in_lake:
                    new_grid[i, j] = Lake
                if grid[i, j] == Tree:  # tree cell
                    # if a tree has at least one burning neighbor and is not within the lake region, it becomes burning
                    if not in_lake and (
                            (i > 0 and grid[i - 1, j] == Burning) or (i < rows - 1 and grid[i + 1, j] == Burning) or (
                            j > 0 and grid[i, j - 1] == Burning) or (j < cols - 1 and grid[i, j + 1] == Burning)):
                        new_grid[i, j] = Burning
                    # if a tree does not have any burning neighbors and is not within the lake region, it ignites with
                    # probability f * (1 - rain_prob)
                    elif not in_lake and np.random.rand() < f * (1 - rain_prob):
                        new_grid[i, j] = Burning
                    # if the tree is not within the lake region and a manual fire has been triggered, it becomes burning
                    elif not in_lake and manual_fire:
                        new_grid[i, j] = Burning
                        manual_fire = False
                elif grid[i, j] == Burning:  # burning cell
                    # a burning cell becomes empty
                    if np.random.rand() < (1 - rain_prob):
                        new_grid[i, j] = Empty
                    else:
                        new_grid[i, j] = Rain
                elif grid[i, j] == Rain:  # rain cell
                    # a rain cell disappears
                    new_grid[i, j] = Empty
                else:  # empty cell
                    # an empty cell becomes a tree with probability p, unless it is within the lake region
                    if not in_lake and np.random.rand() < p:
                        new_grid[i, j] = Tree
                    # if the empty cell is not within the lake region and a manual fire has been triggered,
                    # it becomes burning
                    elif not in_lake and manual_fire:
                        new_grid[i, j] = Burning
                        manual_fire = False
        grid = new_grid
        im.set_data(grid)  # Update the data for the image object
        tree_count = (grid == Tree).sum()
        fire_count = (grid == Burning).sum()
        tree_percent.append(tree_count / (cols * rows))
        fire_percent.append(fire_count / (cols * rows))
        counter += 1
        if counter == 230:
            print(f"Animation finished with {counter} loops.")
            if len(tree_percent) > 0:
                mean_tree_percent = round(np.mean(tree_percent), 2) * 100
                print(f"The mean tree percentage is: {mean_tree_percent}%")
            else:
                print("The tree_percent list is empty, please run the simulation for at least one iteration")
            if len(fire_percent) > 0:
                mean_fire_percent = round(np.mean(fire_percent), 3) * 100
                print(f"The mean fire percentage is: {mean_fire_percent}%")
            else:
                print("The fire_percent list is empty, please run the simulation for at least one iteration")
  
# create a colormap from a list of colors
colormap = ListedColormap(['white', 'green', 'red', 'blue', 'lightblue'])

# Matplotlib animation
fig, ax = plt.subplots()
im = ax.imshow(grid, cmap=colormap, vmin=0, vmax=4)

# Create a play/pause button in the middle of the figure
play_pause_ax = plt.axes([0.46, 0.05, 0.1, 0.05])
play_pause_button = Button(play_pause_ax, 'Pause')

def on_click(event):
    global grid
    # Check if the left mouse button was clicked
    if event.button == 1:
        # Get the row and column indices of the clicked cell
        row, col = int(event.ydata), int(event.xdata)
        # Set the state of the clicked cell to burning
        grid[row, col] = Burning

def play_pause(event):
    global paused
    if not paused:
        paused = True
        play_pause_button.label.set_text('Play')
    else:
        paused = False
        play_pause_button.label.set_text('Pause')

# running 230 times for frames=range(230)
ani = FuncAnimation(fig, update, frames=range(230), fargs=(rain_prob,), repeat=False, interval=100)

# set up the key press event handler
fig.canvas.mpl_connect('button_press_event', on_click)
play_pause_button.on_clicked(play_pause)

# do not show x and y coordinate text
ax.yaxis.set_major_locator(plt.NullLocator())
ax.xaxis.set_major_locator(plt.NullLocator())

# show the animation
plt.show(block=True)

# save the animation
ani.save('fire.gif', writer='pillow', fps=60)
```
![[../../../assets/Coding/fire.gif|400]]


## Plot
```Python
# probability of a tree igniting (f), probability of an empty cell becoming a tree (p) and the probability of rain (r_p)
f = 0.01
p = 0.06
rain_prob = 0.02

# build a square background
cols = 50
rows = 50
print(f'The grid size is {cols}×{rows}!')
print(
    f'The probability of a tree igniting (f) is {f}; of an empty cell becoming a tree (p) is {p}; of rain is {rain_prob}.')

  

# create a grid of cells, with each cell initially set to empty
grid = np.zeros((cols, rows))

# set the start state of the grid, 70% are trees
random.seed(2023)
for i in range(rows):
    for j in range(cols):
        if random.random() < 0.7:  # random.random() used to randomly generates floating-point numbers between (0,1)
            grid[i, j] = 1
        else:
            grid[i, j] = 0

# defines state constants
Empty = 0
Tree = 1
Burning = 2
Rain = 3
Lake = 4

# generate a random lake region
max_lake = round(0.2 * rows)
np.random.seed(2023)
lake_row = np.random.randint(rows)
lake_col = np.random.randint(cols)
lake_size = np.random.randint(0, max_lake)  # side length of the random lake will between 0 and 'max_lake' cells

# Flag to indicate whether the simulation is paused
paused = False
# Flag to indicate whether a manual fire has been triggered
manual_fire = False
# set a counter
counter = 0
# count the cells
tree_percent = []
fire_percent = []

def update(n, rain_prob):
    global grid, paused, manual_fire, counter
    if not paused:
        new_grid = np.copy(grid)
        for i in range(rows):
            for j in range(cols):
                # check if the cell is within the lake region
                in_lake = lake_row <= i < lake_row + lake_size and lake_col <= j < lake_col + lake_size
                if in_lake:
                    new_grid[i, j] = Lake
                if grid[i, j] == Tree:  # tree cell
                    # if a tree has at least one burning neighbor and is not within the lake region, it becomes burning
                    if not in_lake and (
                            (i > 0 and grid[i - 1, j] == Burning) or (i < rows - 1 and grid[i + 1, j] == Burning) or (
                            j > 0 and grid[i, j - 1] == Burning) or (j < cols - 1 and grid[i, j + 1] == Burning)):
                        new_grid[i, j] = Burning
                    # if a tree does not have any burning neighbors and is not within the lake region, it ignites with
                    # probability f * (1 - rain_prob)
                    elif not in_lake and np.random.rand() < f * (1 - rain_prob):
                        new_grid[i, j] = Burning
                    # if the tree is not within the lake region and a manual fire has been triggered, it becomes burning
                    elif not in_lake and manual_fire:
                        new_grid[i, j] = Burning
                        manual_fire = False
                elif grid[i, j] == Burning:  # burning cell
                    # a burning cell becomes empty
                    if np.random.rand() < (1 - rain_prob):
                        new_grid[i, j] = Empty
                    else:
                        new_grid[i, j] = Rain
                elif grid[i, j] == Rain:  # rain cell
                    # a rain cell disappears
                    new_grid[i, j] = Empty
                else:  # empty cell
                    # an empty cell becomes a tree with probability p, unless it is within the lake region
                    if not in_lake and np.random.rand() < p:
                        new_grid[i, j] = Tree
                    # if the empty cell is not within the lake region and a manual fire has been triggered, it becomes burning
                    elif not in_lake and manual_fire:
                        new_grid[i, j] = Burning
                        manual_fire = False
        tree_count = (grid == Tree).sum()
        fire_count = (grid == Burning).sum()
        tree_percent.append(tree_count / (cols * rows))
        fire_percent.append(fire_count / (cols * rows))
        grid = new_grid
        counter += 1
        while counter == 230:
            print(f"Model finished with {counter} loops.")
            if len(tree_percent) > 0:
                mean_tree_percent = round(np.mean(tree_percent), 2) * 100
                print(f"The mean tree percentage is: {mean_tree_percent}%")
            else:
                print("The tree_percent list is empty, please run the simulation for at least one iteration")
            if len(fire_percent) > 0:
                mean_fire_percent = round(np.mean(fire_percent), 3) * 100
                print(f"The mean fire percentage is: {mean_fire_percent}%")
            else:
                print("The fire_percent list is empty, please run the simulation for at least one iteration")
                print("Please look at the plot")
            break

for i in range(230):
    update(i, rain_prob)

# Matplotlib animation
fig, ax = plt.subplots()

# Plot the tree_percent and fire_percent lists on the same axis
ax.plot(range(len(fire_percent)), fire_percent, label='Fire Percentage')
ax.plot(range(len(tree_percent)), tree_percent, label='Tree Percentage')
 

# Add a legend to the plot
ax.legend()

# Add x and y labels
ax.set_xlabel('Steps')
ax.set_ylabel('Percentage')

# Add a title to the plot
ax.set_title('Tree and Fire Percentage by Steps')
 

# Display the plot
plt.show()
```
![[../../../assets/Coding/Plot_output.png|400]]