#Report

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
2. The lilypondStaging is populated when the blocks are run, and the output gets added each time the button/block for saving is used. 
3. To implement the desired feature, I checked how the run button get's it's data so that I can somehow access the blocks in the start which I can add to the staging area and then create the output, but was not able to successfully do that. I tried various approaches which are described in the commit message of my latest code status https://github.com/Daksh/musicblocks/commit/0d81927d9ebb4cf60bfacfa0345332ff0cd806a5