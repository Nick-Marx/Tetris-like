# **Tetris-like Game**

My Live Project assignment for The Tech Academy using Unity/CSharp.
Circumstantially, this turned out to be a solo project.

## **Scene Setup**

To help with outlining the game, I first created 3 scenes: title, main, and end/credit scenes. I decided to use buttons to control the scene transitions and a levelloader prefab to handle a fade-in/out effect.

![](https://github.com/Nick-Marx/The-Tech-Academy-C-Sharp-and-Unity-Projects/blob/main/Unity%20Builds/Tetris-like/README/scenes.gif)

## **Blocks and Block Movement**

The block sprites were created using Aseprite, then imported into Unity and arranged into multiple prefabs. Originally I created each block shape as an entire unit, but changed it into individual 16x16 pixel cells later after thinking it would be easier and a little more versatille this way.

![](https://github.com/Nick-Marx/The-Tech-Academy-C-Sharp-and-Unity-Projects/blob/main/Unity%20Builds/Tetris-like/README/sprite.png)

I created a 1 second timer in the update method and multiplied it by delta time to create a synchronized interval. Next, I made the active block transform its position in the negative vertical at each interval starting from the top of the board.

```
 void Update()
    {
		//update timer
		timer += 1 * Time.deltaTime;

		//quickdrop
		if (Input.GetKey(KeyCode.DownArrow) && timer > BoardLogic.dropAccel)
		{
			gameObject.transform.position -= new Vector3(0, 1, 0);
			timer = 0;
		}
		//constant drop
		else if (timer > BoardLogic.dropTime)
		{
			gameObject.transform.position -= new Vector3(0, 1, 0);
			timer = 0;
		}
	}
```

The active block movement is controlled with the left/right arrow keys and I added in an accelerated fall mechanic that is triggered with the down arrow. The active block can also be rotated using the A and S keys.

A grid was added to help control boundaries and the resting block positions once they become inactive. I then put in a catch to tell when blocks touch, which will deactivate the active block and spawn a new random active block at the top.

![](https://github.com/Nick-Marx/The-Tech-Academy-C-Sharp-and-Unity-Projects/blob/main/Unity%20Builds/Tetris-like/README/blockmovement.gif)

## **Objectives: Win/Lose Conditions**

At this point I wasn't sure how many more features I wanted to add to the game, so I decided to start simple. To win you would have to clear 10 lines, to lose the blocks would have to be stacked to the top of the board.

I struggled with this part, but after a bit of trial and error I found a consistant way to identify if a single horizontal line has been filled and then clear it and move any higher lines down. This likely isn't very optimized code, but for such a small game it doesn't cause any performance issues.

```
public void ClearLines()
    {
        for (int y = 0; y < height; y++)
        {
            if (IsLineComplete(y))
            {
                DestroyLine(y);
                MoveLines(y);
                clearCounter++;
            }
        }
    }
    //move all lines above cleared line down
    void MoveLines(int y)
    {
        for (int i = y; i < height - 1; i++)
        {
            for (int x = 0; x < width; x++)
            {
                if (grid[x, i + 1] != null)
                {
                    grid[x, i] = grid[x, i + 1];
                    grid[x, i + 1].gameObject.transform.position -= new Vector3(0, 1, 0);
                    grid[x, i + 1] = null;
                }
            }
        }
    }
    //destroy filled line
    void DestroyLine(int y)
    {
        for (int x = 0; x < width; x++)
        {
            Destroy(grid[x, y].gameObject);
            grid[x, y] = null;
        }
    }
    //check if a line is filled
    bool IsLineComplete(int y)
    {
        for(int x = 0; x < width; x++)
        {
            if (grid[x, y] == null)
            {
                return false;
            }
        }
        return true;
    }
```

![](https://github.com/Nick-Marx/The-Tech-Academy-C-Sharp-and-Unity-Projects/blob/main/Unity%20Builds/Tetris-like/README/lineclear.gif)

This one also stumped me a little, but I eventually realized a simple solution that seems to work pretty effectively.

![](https://github.com/Nick-Marx/The-Tech-Academy-C-Sharp-and-Unity-Projects/blob/main/Unity%20Builds/Tetris-like/README/blockout.gif)

## **Final Touches**

After clearing the bugs throughout the main mechanics of the game, I went back and made sure that the rules were clearly presented and the game could be replayed or exited without issue.

## **Final Thoughts**

The challeneges presented during this projected had me thinking and looking at problem in new ways. My original plan for how to manage the mechanics in this game didn't quite work out and forced me reconsider my strategy and research a bit more. After I finally got things working, the sense of reward and accomplishment I felt was indescribable. Thank you [The Tech Academy](https://www.learncodinganywhere.com/) for giving me this opportunity and helping me succeed.


