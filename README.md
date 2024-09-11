# xctrace General Commands Manual

#### NAME

<code>xctrace</code> – Record, import, export and symbolicate Instruments .trace files.

#### SYNOPSIS

     xctrace [command [options]]

     xctrace help [command]

     xctrace record [--output path] [--append-run] [--template path | name] [--device name | UDID]
             [--time-limit time [ms|s|m|h]] [--all-processes | --attach pid | name | --launch command [arguments]]
             [--target-stdin - | file] [--target-stdout - | file] [--env VAR=value] [--package file] [--no-prompt]
             [--notify-tracing-started name]

     xctrace import [--input file] [--output path] [--template path | name] [--package file]

     xctrace export [--input file] [--output path] [--toc | --xpath expression]

     xctrace remodel [--input file] [--output path] [--package file]

     xctrace symbolicate [--input file] [--output path] [--dsym file]

     xctrace list [devices | templates | instruments]

#### DESCRIPTION

<code>xctrace</code> is used to record new Instruments.app .trace files using the specified recording template.  It also allows
     for converting supported converting files of supported input types  into .trace files and exporting data from .trace
     files to parseable formats like xml.

Available commands and their options:

     help        General help or help specific to command argument

     record      Perform a new recording on the specified device and target with the given template. Resulting .trace file
                 can be viewed in Instruments.app or exported using xctrace.  Recording can target all processes, attach
                 to an existing process or launch a new one.

                 --output path     Save the trace to the specified path.  A path to an existing trace file can be used if
                                   --append-run is specified. In this case the provided template will be ignored and the
                                   template in the existing trace will be used.  If the passed path is a directory, a
                                   uniquely named file will be created in it.  If the path contains the extension .trace,
                                   a new trace will be created with that name at the specified path or working directory.

                 --append-run      Allows to target an existing trace file. Specifying it causes new run to be appended.
                                   New run is recorded with the template that this existing trace file was initially
                                   recorded with.

                 --template path | name
                                   Records using given trace template name or path. If name is specified, template needs
                                   to be either bundled with an installed Instruments Package or located in the
                                   Application Support directory for Instruments. To retrieve available templates, run
                                   xctrace list templates.

                 --instrument name
                                   Adds Instrument with the specified name to the recording configuration. To retrieve
                                   available Instruments, run xctrace list instruments.

                 --device name | UDID
                                   Record on a device with the given name or UDID. If not specified, then local device is
                                   chosen for the recording.  If the word simulator is included, devices of simulator
                                   types will be preferred.  To get list of available devices, run xctrace list devices.

                 --time-limit time [ms|s|m|h]
                                   Limits the recording to the specified time period. Example: "--time-limit 5s" or
                                   "--time-limit 1m"

                 --window duration [ms|s|m]
                                   Configure trace to operate in windowed mode, where trace buffer acts as a ring buffer,
                                   removing old events to make room for new ones. Example: "--window 5s"

                 --all-processes   Record all processes running on the system.

                 --attach pid | name
                                   Attach and record running process with the given name or pid.

                 --launch -- command [arguments]
                                   Launch and record process with the given name or path and provided arguments list. This
                                   option should be passed as the last one in the xctrace command invocation.

                 --package file    Install the given Instruments Package temporarily for the duration of a command

                 --target-stdin - | file
                                   Redirects standard input stream of a launched process in case if target device is set
                                   to the local Mac.  Can be pointing to a file or be specified as - , which causes
                                   standard input of xctrace to be passed into the standard input of a target process.

                 --target-stdout - | file
                                   Redirects standard output stream of a launched process.  Can be pointing to a file or
                                   be specified as - , which causes standard output of a target process to be to be
                                   redirected into the standard output of xctrace.

                 --env VAR=value   Sets environment variable for a launched process. Example: "--env PATH=/tmp"

                 --no-prompt       Skip any prompts that would be otherwise presented (like privacy warnings)

                 --notify-tracing-started name
                                   Send Darwin notification with this name when a recording has started.


     import      If the specified file is of a support input format, imports the file into an Instruments .trace file
                 using the specified template. This file can later be viewed using Instruments.app or exported by using
                 xctrace export command.

                 --input file      Import data from a supported file format at the given path.

                 --output path     Save the trace to the specified path. Existing trace files cannot be targeted.  If the
                                   passed path is a directory, a uniquely named file will be created in it.  If the path
                                   contains the extension .trace, a new trace will be created with that name at the
                                   specified path or working directory.

                 --template path | name
                                   Imported using given trace template name or path. Only data that is needed by
                                   Instruments contained in the template will be imported. If name is specified, template
                                   needs to be either bundled with an installed Instruments Package or located in the
                                   Application Support directory for Instruments. To retrieve available templates, run
                                   xctrace list templates.

                 --instrument name
                                   Adds Instrument with the specified name to the import configuration. To retrieve
                                   available Instruments, run xctrace list instruments.

                 --package file    Install the given Instruments Package temporarily for the duration of a command.


     export      Perform a new recording on the specified device and target with the given template. Resulting .trace file
                 can be viewed in Instruments.app or exported using xctrace.  Recording can target all processes, attach
                 to an existing process or launch a new one.

                 --input file      Export data from .trace file at the given path.

                 --output path     Save output of the export operation to the given path. If not specified, then textual
                                   output is being written to the xctrace standard output.

                 --toc             Export the table of contents of a trace file. This will contain overview of content and
                                   exportable entities from the given trace file.

                 --xpath expression
                                   Perform given XPath query on the table of contents to select entities to export from
                                   the given trace file. Selected entities will be present in the export command output.

                 --har             Export data as the HTTP Archive file if the trace run contains the HTTP Traffic
                                   Instrument.


     remodel     Remodel trace using currently installed packages and their modelers.

                 --input file      Remodel data from .trace file at the given path.

                 --output path     Save remodeled .trace file to the given path. .

                 --package file    Install the given Instruments Package temporarily for the duration of a command.


     symbolicate
                 Symbolicate trace file using supplied debug symbols (dSYM).

                 --input file      Symbolicate addresses in .trace file at the given path.

                 --output path     Save symbolicated .trace file to the given path. If not specified, the symbolicated
                                   .trace will be saved to its original location (input path).

                 --dsym path       Use symbol data from dSYM at given path. Path can be a dSYM or a directory to search
                                   recursively for dSYMs. If not specified, the tool will make its best effort to locate
                                   dSYMs (it may take a while).


     list devices
                 Lists all of non-host devices that can be used as a target for a new recording.

     list templates
                 Lists all of templates that are known to xctrace.  These include Instruments.app bundled templates,
                 templates from installed Instruments Packages and custom templates installed into Application Support
                 directory for Instruments.app.

     list instruments
                 Lists all of the Instruments available to select as part of the record or import workflow.

     Global options:

     --quiet     Make terminal output less verbose

#### SEE ALSO

Instruments.app may be used to perform trace recordings in a graphical environment and may also be used to open trace
     documents created by xctrace.

#### EXAMPLES

Import a logarchive file to the trace file with a unique name using a MyCustomTemplate template:
     
           % xctrace import --input system_logs.logarchive --template 'MyCustomTemplate'

Import a ktrace file into a new document created from the template with name MyCustomTemplate and adding the instrument from /tmp/PackageToLoad.instrdst and save the resulting document as output.trace:
     
           % xctrace import --input trace001.ktrace --template 'MyCustomTemplate' --package '/tmp/PackageToLoad.instrdst' --output output.trace

Export a table of contents (toc) of the input.trace file to standard output:

           % xctrace export --input input.trace --toc

Export recorded data from a table with my-table-schema schema in the first run of the input.trace file to standard output:

           % xctrace export --input input.trace --xpath '/trace-toc/run[@number="1"]/data/table[@schema="my-table-schema"]'

Export UUID, binary path, load address and architecture for each binary image contained in the first run of the input.trace and save as output.xml:

           % xctrace export --input input.trace --output output.xml --xpath '/trace-toc/run[@number="1"]/processes'

Export UUID, binary path, load address and architecture for each binary image used by the process named my-process-name in the first run of the input.trace file to standard output:

           % xctrace export --input input.trace --xpath '/trace-toc/run[@number="1"]/processes/process[@name="my-process-name"]'

Start recording all processes on the local Mac device using the Time Profiler template and automatically stop the recording after 5s:

           % xctrace record --all-processes --template 'Time Profiler' --time-limit 5s

Start a new recording by attaching to the process with name Trailblazer on the connected device Chad's iPhone using the template Time Profiler:

           % xctrace record --template 'Time Profiler' --device-name 'Chad's iPhone' --attach 'Trailblazer'

Start Metal System Trace template recording on a simulator device named iPhone SE Simulator, capturing all existing processes:

           % xctrace record --template 'Metal System Trace' --device-name 'iPhone SE Simulator' --all-processes

Start Time Profiler template recording on a local Mac device, launching and profiling binary at /tmp/tool path with arg1 arg2 arguments, output of the binary gets redirected to the standard output:

           % xctrace record --template 'Time Profiler' --target-stdout - --launch -- /tmp/tool arg1 arg2

Symbolicate the input.trace file using debug information from a SomeLibrary.dSYM file:

           % xctrace symbolicate --input input.trace --dsym SomeLibrary.dSYM

Symbolicate the input.trace file by trying automatically locate debug information:

           % xctrace symbolicate --input input.trace

macOS                                                 January 3, 2023

----
_Retrieved with <code>man xtrace</code> command on September 10, 2024._
