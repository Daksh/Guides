#Report

##Status
I will like to start by saying that I know that this does not work properly. The last attempt, sort of fixes the problem in a bad way. Down in this report, I have described what I have tried.

##Redundant code
It was successfully removed. And the code transferred to a function so that it can be called from both the places.
[Corresponding GitHub Commit](https://github.com/Daksh/musicblocks/commit/b3dec5865dadd136e119985f6bd4673365366c16)
The code had a slight modification for both the calls, so I introduced a new argument which takes care of that change

##File Name
A prompt was added to get the file name from the user which is to be saved.
The following code does the required:
```javascript
var lyfilename = prompt("Please enter the file name", "lilypondexport.ly");
if (lyfilename != null){
if(lyfilename != "")
  doSaveLilypond(logo, lyfilename);
else
  doSaveLilypond(logo, "lilypondexport.ly");
}
```
`null` represents the action of closing, in which case it does not save
` ` if the user enters no name, the default name `lilypondexport.ly` gets used
Otherwise the specified name is used

##The other feature
As it was suggested by walter, I have been trying to lean JavaScript and implement the feature that you asked me to, ever since that was told to me. I found out a good number of things like:  

1. The data is stored in a 'logo' structure which has "lilypondStaging" and "lilypondOutput" 
<img src="\images\1.png"></img>
that is what can be seen in the logs, to get that I have used the `console.log(logo)` statement in the [savely function](https://github.com/Daksh/musicblocks/commit/0d81927d9ebb4cf60bfacfa0345332ff0cd806a5#diff-c9771817b0b1e7ef2e9185f91a62b4cfR2475)
2. The `lilypondStaging` is populated, i.e. it gets the data when the blocks are run, and the output gets added each time to the `lilypondOutput` when the button/block for saving is used. 
3. To implement the desired feature, I checked how the run button get's it's data so that I can somehow access the blocks in the start which I can add to the staging area and then create the output, but was not able to successfully do that. 

###Attempts

Commmit Message: 

Trying to asynchronously:
1. Clear the staging area, notes, and output of Lilypond
2. Run the blocks from the runLogoCommands function, which currently might not be silent, but first trying to implement this, as I read that function but could not find a way to make it silent. Excluding the GUI commands might do it but I don't have much clue on how to do that. Read the code and tried Googling too
3. Save the file

But the problem as of now is that the #2 does not end but the 3 starts, which does not add the content in the saved file.

To eliminate this problem I tried to use callback functions so that it starts only once the function call ends, but it is in itself calling more synchornous functions so the callback comes before it actually ends.

To solve this problem I tried looking at the Timeout function which is supposed to wait in given milliseconds time for the return, but I could not implement or understand that properly

Related Code: https://github.com/Daksh/musicblocks/commit/0d81927d9ebb4cf60bfacfa0345332ff0cd806a5

------

After that I realized that I could further pass the callbackFunction down the chain to the place where it finally executes the required steps. So I passed it [from the saveLilyPondButton](https://github.com/Daksh/musicblocks/blob/bc847f3a24fd3695325a3ada83de64f1c56e7725/js/activity.js#L317) to the [runLogoCommands](https://github.com/Daksh/musicblocks/blob/bc847f3a24fd3695325a3ada83de64f1c56e7725/js/logo.js#L442) to  the [runFromBlock](https://github.com/Daksh/musicblocks/blob/bc847f3a24fd3695325a3ada83de64f1c56e7725/js/logo.js#L593) and from there to the runFromBlockNow and from there to saveLocally and finally callback there. I tried checking on each step too and all of them together too. 

But it is still returning before the process completes.
Corresponding [Commit](https://github.com/Daksh/musicblocks/commit/bc847f3a24fd3695325a3ada83de64f1c56e7725)

--------

After that not working, I tried to implement the setTimeout Function which was made to pause 5 seconds before calling.

Commit Message: This is ineffective and not the required solution, but it works. Which indicates that the notes are getting added when they are actually played, which is when the 'notes to play' is printed in the console. This line is on L1951 in logo.js but over there I cannot see when/how is it adding that note to the lilypondStaging

[Corresponding Commit](https://github.com/Daksh/musicblocks/commit/d09044e75cb5ad52bb65d829670d9a694009596b)

--------

Other than these I tried looking at how the asynchornous calls could be achieved by looking at `deffered` and `when done` but there was not much luck there.

I looked at the `save as .tb` which instantly saves the file as `.tb` but the problem over there is that, in that the function looks for all the blocks expcept the ones in trash and then uses a map/graph to add the links between the blocks that exist. (I do not have much knowledge of graphs/maps either)

But in the lilypond save, we do not need that. We rather need the notes that have been played because that is all that we have to export. And those get added to the `lilypondStaging` when the program is `Run`->`runlogoCommands`->`runFromBlock`->`runFromBlockNow` 

The `runFromBlockNow` is in itself a huge function. But if we just look at the notes playing here, it gets processed in one of the cases of the switch case inside that function, but even there I could not find a reference which was adding the notes to the lilypondStaging.

Further to conclude this, I would like to say that I am not familiar with JavaScript, so even though I found the whole workflow that I did, I am not able to make the changes to the code so easily. And the ones that I did make were each after a few hours of Googling on how to do that, for example the callback system and the timeout wait.