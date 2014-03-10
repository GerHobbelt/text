# text

A [RequireJS](http://requirejs.org)/AMD loader plugin for loading text
resources and exporting separate strings separated on <!-- export name="someProperty" -->.

Known to work in RequireJS, but should work in other AMD loaders that support
the same loader plugin API.

## Docs

This is a fork of the Text plugin. All the documentation of the text plugin still applies.
See the [RequireJS API text section](http://requirejs.org/docs/api.html#text).

## Usage



You can specify an export text file resource as a dependency like so:

```javascript
require(["text!some/module.html!export"],
    function(html) {
        //the html variable will be an object of the
        //some/module.html file, with all the text
        //between <!-- export name="someExportName" -->
        //and the next <!-- export --> comment
        //accessible like 'html.someExportName'
        //If no <!-- export --> is found, the
        //plugin will default to html as a string
    }
);
```
The <!-- export --> comment is found through regex, so nested objects are not possible. It ignores exports with no property name, but any <!-- export --> comment will end the previous export.
The regex works like [this](http://regexpal.com/?flags=g&regex=(%3C!--%5Cs*%3Fexport%5Cs%2B%3Fname%5B%5C%3A%5C%3D%5D(%5B%5C%27%5C%22%5D)%5Ba-zA-Z%5D%2B%3F%5Cw*%3F%5C2%5Cs*%3F--%3E)%5B%5Cs%5CS%5D%2B%3F((%3F%3D%3C!--%5Cs*%3Fexport(%5Cs%2B%3Fname%5B%5C%3A%5C%3D%5D(%5B%5C%27%5C%22%5D)%5Ba-zA-Z%5D%2B%3F%5Cw*%3F%5C5)%3F%5Cs*%3F--%3E)%7C(%3F%3A(%3F!%5B%5CS%5Cs%5D)))&input=%3Cbody%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22PatientNameTh%22%20--%3E%0A%20%20%20%20%3Cth%20data-fieldname%3D%22PatientPerson%22%3E%0A%20%20%20%20%20%20%20%20Name%20%3Cspan%20data-bind%3D%22attr%3A%20%7B%20class%3A%20sortField()%20%3D%3D%20%27PatientPerson%27%20%3F%20%27inline-block%27%20%3A%20%27hide%27%20%7D%22%3E%3C%2Fspan%3E%0A%20%20%20%20%3C%2Fth%3E%0A%0Aother%20stuff%0A%0A%20%20%20%20%3C!--%20export--%3E%0A%20%20%20%20%3Ctd%3E%0A%20%20%20%20%20%20%20%20%3Cspan%20class%3D%22glyphicon%20glyphicon-expand%22%3E%3C%2Fspan%3E%0A%20%20%20%20%20%20%20%20%3Ca%20data-bind%3D%22click%3A%20function%20()%20%7B%20%24root.loadReportSummary(PatientPerson.ID())%20%7D%22%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%3Cspan%20data-bind%3D%22text%3A%20%24data.PatientPerson%20%26%26%20%24data.PatientPerson.FullName%22%3E%3C%2Fspan%3E%0A%20%20%20%20%20%20%20%20%3C%2Fa%3E%0A%20%20%20%20%3C%2Ftd%3E%0A%3C!--%20export%20--%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22StudyTypeTh%22%20--%3E%0A%20%20%20%20%3Cth%20data-fieldname%3D%22StudyType%22%3EStudy%20Type%20%3Cspan%20data-bind%3D%22attr%3A%20%7B%20class%3A%20sortField()%20%3D%3D%20%27StudyType%27%20%3F%20%27inline-block%27%20%3A%20%27hide%27%20%7D%22%3E%3C%2Fspan%3E%3C%2Fth%3E%0A%20%20%20%20%3C!--export%20%20%20%20%20name%3D%22StudyTypeTd%22--%3E%0A%20%20%20%20%3Ctd%20data-bind%3D%22text%3A%20%24data.StudyType%22%3E%3C%2Ftd%3E%0A%20%20%20%20%3C!--%20export%20--%3E%0A%20%20%20%20%3C!--%20export%20--%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22ServiceDateTh%22%20--%3E%0A%20%20%20%20%3Cth%20data-fieldname%3D%22ServiceDate%22%3EService%20Date%3Cspan%20data-bind%3D%22attr%3A%20%7B%20class%3A%20sortField()%20%3D%3D%20%27ServiceDate%27%20%3F%20%27inline-block%27%20%3A%20%27hide%27%20%7D%22%3E%3C%2Fspan%3E%3C%2Fth%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22ServiceDateTd%22--%3E%0A%20%20%20%20%3Ctd%20data-bind%3D%22text%3A%20%24data.ServiceDate%22%3E%3C%2Ftd%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22ExportSummaryTh%22%20--%3E%0A%20%20%20%20%3Cth%3EExport%20Summary%3C%2Fth%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22ExportSummaryTd%22--%3E%0A%20%20%20%20%3Ctd%3E%3Ca%20data-bind%3D%22click%3A%20function%20(data)%20%7B%20%24root.exportReportSummary(data%2C%20PatientPerson.ID%2C%20SummaryID%2C%20!StudyExported())%20%7D%22%3EView%3C%2Fa%3E%3C%2Ftd%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22PrintAllReportsTh%22%20--%3E%0A%20%20%20%20%3Cth%3EPrint%20All%20Reports%3C%2Fth%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22PrintAllReportsTd%22--%3E%0A%20%20%20%20%3Ctd%3E%3Ca%20data-bind%3D%22click%3A%20function%20(data)%20%7B%20%24root.printAllReports(%27%2FPrintReports%2FReports%3FsummaryID%3D%27%20%2B%20SummaryID)%20%7D%22%3EPrint%3C%2Fa%3E%3C%2Ftd%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22AssignStudyTh%22%20--%3E%0A%20%20%20%20%3Cth%20data-fieldname%3D%22HasAssigned%22%3EAssign%3Cspan%20data-bind%3D%22attr%3A%20%7B%20class%3A%20sortField()%20%3D%3D%20%27HasAssigned%27%20%3F%20%27inline-block%27%20%3A%20%27hide%27%20%7D%22%3E%3C%2Fspan%3E%3C%2Fth%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22AssignStudyTd%22--%3E%0A%20%20%20%20%3Ctd%3E%3Ca%20data-bind%3D%22visible%3A%20!%24data.StudyConfirmed%2C%20click%3A%20function%20()%20%7B%20%24root.assignStudy(%24data.SummaryID)%20%7D%22%3EAssign%3C%2Fa%3E%3C%2Ftd%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22FellowNameTh%22%20--%3E%0A%20%20%20%20%3Cth%20data-fieldname%3D%22FellowPerson%22%3EFellow%20Name%3Cspan%20data-bind%3D%22attr%3A%20%7B%20class%3A%20sortField()%20%3D%3D%20%27FellowPerson%27%20%3F%20%27inline-block%27%20%3A%20%27hide%27%20%7D%22%3E%3C%2Fspan%3E%3C%2Fth%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22FellowNameTd%22--%3E%0A%20%20%20%20%3Ctd%20data-bind%3D%22text%3A%20%24data.FellowPerson%20%26%26%20%24data.FellowPerson.FullName%22%3E%3C%2Ftd%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22AttendingNameTh%22%20--%3E%0A%20%20%20%20%3Cth%20data-fieldname%3D%22AttendingPerson%22%3EAttending%20Name%3Cspan%20data-bind%3D%22attr%3A%20%7B%20class%3A%20sortField()%20%3D%3D%20%27AttendingPerson%27%20%3F%20%27inline-block%27%20%3A%20%27hide%27%20%7D%22%3E%3C%2Fspan%3E%3C%2Fth%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22AttendingNameTd%22--%3E%0A%20%20%20%20%3Ctd%20data-bind%3D%22text%3A%20%24data.AttendingPerson%20%26%26%20%24data.AttendingPerson.FullName%22%3E%3C%2Ftd%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22StudyConfirmedTh%22%20--%3E%0A%20%20%20%20%3Cth%20data-fieldname%3D%22StudyConfirmed%22%3EStudy%20Confirmed%3Cspan%20data-bind%3D%22attr%3A%20%7B%20class%3A%20sortField()%20%3D%3D%20%27StudyConfirmed%27%20%3F%20%27inline-block%27%20%3A%20%27hide%27%20%7D%22%3E%3C%2Fspan%3E%3C%2Fth%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22StudyConfirmedTd%22--%3E%0A%20%20%20%20%3Ctd%20class%3D%22text-center%22%3E%3Cspan%20class%3D%22glyphicon%20glyphicon-ok%22%20data-bind%3D%22visible%3A%20%24data.StudyConfirmed%22%20%2F%3E%3C%2Ftd%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22StudyExportedTh%22%20--%3E%0A%20%20%20%20%3Cth%20data-fieldname%3D%22StudyExported%22%3EStudy%20Exported%3Cspan%20data-bind%3D%22attr%3A%20%7B%20class%3A%20sortField()%20%3D%3D%20%27StudyExported%27%20%3F%20%27inline-block%27%20%3A%20%27hide%27%20%7D%22%3E%3C%2Fspan%3E%3C%2Fth%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22StudyExportedTd%22--%3E%0A%20%20%20%20%3Ctd%20class%3D%22text-center%22%3E%3Cspan%20class%3D%22glyphicon%20glyphicon-ok%22%20data-bind%3D%22visible%3A%20%24data.StudyExported()%22%20%2F%3E%3C%2Ftd%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22eMailViewTh%22%20--%3E%0A%20%20%20%20%3Cth%20data-fieldname%3D%22EmailLogID%22%3EReferral%3Cspan%20data-bind%3D%22attr%3A%20%7B%20class%3A%20sortField()%20%3D%3D%20%27EmailLogID%27%20%3F%20%27inline-block%27%20%3A%20%27hide%27%20%7D%22%3E%3C%2Fspan%3E%3C%2Fth%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22eMailViewTd%22--%3E%0A%20%20%20%20%3Ctd%3E%3Ca%20data-bind%3D%22visible%3A%20%24data.EmailLogID%20!%3D%200%22%20class%3D%22show-remote-modal%22%20data-target%3D%22%23myModal%22%20data-title%3D%22Referral%22%20data-fullscreen%3D%22true%22%3EPrint%3C%2Fa%3E%3C%2Ftd%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22Flag0Th%22%20--%3E%0A%20%20%20%20%3Cth%20data-fieldname%3D%22Flag0%22%3EStudy%20Billed%3Cspan%20data-bind%3D%22attr%3A%20%7B%20class%3A%20sortField()%20%3D%3D%20%27Flag0%27%20%3F%20%27inline-block%27%20%3A%20%27hide%27%20%7D%22%3E%3C%2Fspan%3E%3C%2Fth%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22Flag0Td%22--%3E%0A%20%20%20%20%3Ctd%3E%3Ca%20href%3D%22%23%22%20data-bind%3D%22text%3A%20%24data.Flag0()%20%3F%20%27Yes%27%20%3A%20%27No%27%2C%20css%3A%20%7B%20stable%3A%20%24data.Flag0()%2C%20critical%3A%20!%24data.Flag0()%20%7D%2C%20click%3A%20setUnsetStudyBilled%22%20data-loader-message%3D%22updating...%22%3E%3C%2Fa%3E%3C%2Ftd%3E%0A%3C!--%20export%20--%3E%0A%20%20%20%20%3C!--%20export%20name%3A%22Flag0Td%22--%3E%0A%20%20%20%20%3Ctd%3E%3Ca%20href%3D%22%23%22%20data-bind%3D%22text%3A%20%24data.Flag0()%20%3F%20%27Yes%27%20%3A%20%27No%27%2C%20css%3A%20%7B%20stable%3A%20%24data.Flag0()%2C%20critical%3A%20!%24data.Flag0()%20%7D%2C%20click%3A%20setUnsetStudyBilled%22%20data-loader-message%3D%22updating...%22%3E%3C%2Fa%3E%3C%2Ftd%3E%0A%3C%2Fbody%3E).

It then removes the <!-- export --> comments from the text. All spaces are preserved.

It also works with the strip command

```javascript
require(["text!some/module.html!export!strip"],
    function(html) {
        //Any text in the header will be removed
        //including any <!-- export --> comments
    }
);
require(["text!some/module.html!strip!export"],
    function(html) {
        //Feel free to change the order of the
        //plugin tags
    }
);
require(["text!some/module.html!strip"],
    function(html) {
        //Again this is a fork, so this will work as normal
    }
);
```
The r.js build will inline all the text like so

```
require(["text!some/module.html!export"],
    function(html) {
       {'someProperty':'text between\n\rnext export comment','nextProperty':'text in the next export with a name'}
    }
);
```

## History
This plugin is a fork of the text plugin from [requirejs repo](https://github.com/requirejs/text)
If this project is out of date with the original requirejs text plugin, submit a pull request or let me know.
