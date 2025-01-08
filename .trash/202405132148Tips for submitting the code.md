# Tips for submitting the code
### Tip 1: Send as ==*less code*== as possible
The thousands lines of code on a code review are a PIA for a code reviewer cause it makes the process a nightamre. One of the basic reasons for this is the difficulty of <mark style="background: #D2B3FFA6;">cathcing the bugs</mark> during the analysis.
> The author says to keep PR/MR between `10-100` lines of code.

### Tip 2: Leave an informative description of MR
Reviewer will keep some context about your changes and updates in code during the review. ==It will prevent from unceartin treads about meaning and usability of written code.==
It’s a good practice to show as much clear information as possible. It might be:
- A simple line `fix: add auth method`
- A complex implementation the requires a comprehensive description
- A link to a task in Task Tracker (i.e. *Notion*)
- A screenshot of before and after changes
- <mark style="background: #D2B3FFA6;">A prompt that generated the code</mark>
### Tip 3: Comment on neccessary parts of diffs that appeared in MR
Better to leave the comment in a code where some changes happend so code reviewer knows that the changes are might not be obvious but still usefull in this MR. ==So the comments are NOT for a code base but for a code reviewer.==