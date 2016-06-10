---
layout: docs
title:  Specifying stream properties
description:
weight: 243
---

# Specifying stream properties
---

## Refactoring
Why do you have to click a button and bring up a new dialog to rename a stream or an operator? Why not just type away in the field showing the name?
Because changing a name involves a refactoring process, meaning that all references to that name must be found and changed too. Streams Studio needs a signal to know when to kick off this process; in this case, you provide that signal by clicking OK.

1. To assign a name and type to a stream, select the stream (dashed arrow) connecting FileSource_2 and Filter_3. (Sometimes you need to try a few times before the cursor catches the stream, instead of selecting the enclosing main composite.)

    The Properties view, which you used earlier to create LocationType, now shows Stream properties. Reposition and resize the view if necessary so it doesn’t obscure the graph you’re editing. (If you closed the Properties view, double-click the stream to reopen it.)

1. Descriptive stream names are preferable to the placeholder names generated by the editor.

    * In the Properties view (General tab), click Rename….

    * In the Rename dialog, under Specify a new name: type Observations, and click OK.

1. Specify the stream schema. You can do that in the Properties view, but since you have created a type for this, you can also just drag and drop it in the graphical editor, like any other object.

    In the graphical editor, clear the palette filter by clicking the eraser button   next to where you typed fil earlier; this makes all the objects visible again. Under Current Graph, expand Schemas. This shows the LocationType type, along with the names of the two streams in the graph. Select LocationType and drag it into the graph; drop it onto the Observations stream (the one between FileSource_2 and Filter_3). Make sure the stream’s selection handles turn green as you hover before you let go, as shown below.

    If you open the Properties view to the Schema tab, it now shows a Type of LocationType, and <extends> as a placeholder under Name. This indicates that the stream type does not contain some named attribute of type LocationType, but instead inherits the entire schema with its attribute names and types.

1. Using the same drag and drop technique, assign the LocationType type to the other stream, between Filter_3 and FileSink_1. Select that stream so its properties show in the Properties view.

1. In the Properties view, General tab, rename the stream to Filtered. (Click Rename…, etc.)

<div class="alert alert-info" role="alert">
<h4><b>Note</b></h4>
There is still an error indicator on FileSink_1 and, because of that, on the main composite, too. This is expected, because you have not yet told the FileSink operator what file to write. You also need to provide details for the other operators.
</div>

# Specifying operator properties

Operator identifiers and aliases: confusing?
SPL automatically assigns names (Identifiers) to operators by using the name(s) of the output stream(s). It also provides for aliases to use as identifiers instead, giving the developer more control. The graphical editor automatically assigns a generated alias to every operator; but in case of a single output stream, using the stream name is usually fine, so we prefer to omit the alias. For multiple output streams, it’s a good idea to provide a more descriptive alias. When there is no output stream (as for the FileSink), an alias is mandatory.


1. In the graphical editor, select FileSink_1. In the Properties view, click the Param tab. This shows one mandatory parameter, file, with a placeholder value of parameterValue (not a valid value, hence the error marker). Click on the field that says parameterValue and type "filtered.cars" (with the double quotes). Press Enter.

    Note that this is a relative path (it doesn’t start with “/”), so this file will go in the data subdirectory of the current project, as specified by default for this application.

1. You need two more parameters. Click Add…; in the Select parameters dialog, check format and quoteStrings (you may have to scroll down to find it); click OK. For the value of format, enter csv (no quotes: this is an enumerated value). For the value of quoteString, enter false (no quotes: this is a boolean value).

1. The FileSource operator needs to know what file to read. In the graphical editor, select the FileSource_2 operator. In the Properties view (Param tab), click Add…; in the Select parameters dialog, select file and format, and click OK. In the value for file, enter "/home/streamsadmin/data/all.cars" (with quotes and exactly as shown here, all lowercase); for format, enter csv. The two displays should look as follows:

    <img width="80%" src="/tutorials/images/Lab1/3.jpg"/>

1. You have to tell the Filter operator what to filter on; without a filter condition, it will simply copy every input tuple to the output. In the graphical editor, select Filter_3. In the Properties view (Param tab), click Add…; in the Select parameters dialog, select filter and click OK.

    In the value field, enter the boolean expression id in ["C101","C133"] to indicate that only tuples for which that expression evaluates to true should be passed along to the output. (The expression with the key word in followed by a list evaluates to true only if an element of the list matches the item on the left.)

1. Save. The error markers disappear: you have a valid application.

1. There are just a few more things to clean up.

    a. Select the FileSink operator again. Go back to the General tab in the Properties view; rename the operator in the same way you renamed the two streams earlier; call it Writer.

    b. Select the FileSource operator and rename it. This time, simply keep the name blank. Observe how the Identifier changes to Observations, the name of the output stream.

    c. Also rename the Filter operator by blanking out the alias. In the graph it will show up as Filtered (the name of the output stream).

    d. Save. Dismiss the floating Properties view.

### Check your results

The Properties view may have been obscuring this, but each time you saved the graph, a build was started to compile the application; the progress messages are shown in the Console view (in the SPL Build console). If you scroll back (you may also want to enlarge or maximize the Console view), you will see some builds that terminated on errors, with messages in red. The last build should have completed without any errors.

 {% include nextPageFinder.html context=page.url %}
 