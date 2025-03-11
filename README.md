21 Practical Statements :

1) Question: Log File Creator

$file = "script_log.txt"
FileWrite($file, @YEAR & "-" & @MON & "-" & @MDAY & " " & @HOUR & ":" & @MIN & ":" & @SEC & " - Script executed successfully." & @CRLF)
MsgBox(0, "Log Updated", "Log updated successfully.")

(check file in c drive/users/pc/documents)


2) Question: Custom HotKey Actions

; Assign hotkeys to specific actions
; HotKeySet binds a key combination to a custom function

HotKeySet("^!n", "OpenNotepad") ; Ctrl + Alt + N
HotKeySet("^!b", "OpenFolder")  ; Ctrl + Alt + B
HotKeySet("^!e", "ExitScript")  ; Ctrl + Alt + E

; Infinite loop to keep the script running
While 1
    Sleep(100) ; Prevent high CPU usage
WEnd

; Function to open Notepad
Func OpenNotepad()
    Run("notepad.exe") ; Run Notepad
EndFunc

; Function to open a specific folder
Func OpenFolder()
    Run("explorer.exe C:\Your\Folder\Path") ; Replace with your desired folder path
EndFunc

; Function to exit the script
Func ExitScript()
    Exit ; Stops the script
EndFunc



3) Question: Calculator

#include <GUIConstantsEx.au3>

Global $sFirstNum = "", $sOperator = "", $bNewInput = False

GUICreate("Calculator", 260, 230)
Global $idEdtScreen = GUICtrlCreateEdit("0", 8, 2, 239, 23, 0x0800)

; Define button labels
Global $aBtns = ["7", "8", "9", "/", "4", "5", "6", "*", "1", "2", "3", "-", "0", ".", "=", "+", "C"]
Global $aIDs[UBound($aBtns)]

; Create buttons dynamically in a 4x5 grid layout
For $i = 0 To UBound($aBtns) - 1
    $x = 54 + Mod($i, 4) * 39
    $y = 73 + Int($i / 4) * 33
    $aIDs[$i] = GUICtrlCreateButton($aBtns[$i], $x, $y, 36, 29)
Next

GUISetState()

While True
    $msg = GUIGetMsg()
    If $msg = $GUI_EVENT_CLOSE Then Exit

    ; Handle button clicks
    For $i = 0 To UBound($aIDs) - 1
        If $msg = $aIDs[$i] Then
            Switch $aBtns[$i]
                Case "0" To "9", "."
                    If $bNewInput Then
                        GUICtrlSetData($idEdtScreen, $aBtns[$i])
                        $bNewInput = False
                    Else
                        Local $sCurrent = GUICtrlRead($idEdtScreen)
                        GUICtrlSetData($idEdtScreen, $sCurrent & $aBtns[$i])
                    EndIf
                Case "+", "-", "*", "/"
                    $sFirstNum = GUICtrlRead($idEdtScreen)
                    $sOperator = $aBtns[$i]
                    $bNewInput = True
                Case "="
                    If $sFirstNum <> "" And $sOperator <> "" Then
                        Local $sSecondNum = GUICtrlRead($idEdtScreen)
                        Local $sResult = Execute($sFirstNum & $sOperator & $sSecondNum)
                        GUICtrlSetData($idEdtScreen, $sResult)
                        $sFirstNum = ""
                        $sOperator = ""
                        $bNewInput = True
                    EndIf
                Case "C"
                    GUICtrlSetData($idEdtScreen, "0")
                    $sFirstNum = ""
                    $sOperator = ""
                    $bNewInput = False
            EndSwitch
        EndIf
    Next
WEnd


4) Question : Basic GUI File Opener

#include <GUIConstantsEx.au3>
#include <WindowsConstants.au3>
#include <File.au3>

; Main GUI window
$hGUI = GUICreate("File Path Finder", 400, 120)
GUISetBkColor(0xF0F0F0, $hGUI) ; Set background color

; Path entry input
$inputPath = GUICtrlCreateInput("", 20, 30, 360, 20)

; Buttons
$btnBrowse = GUICtrlCreateButton("Browse", 20, 70, 80, 30)
$btnClear = GUICtrlCreateButton("Clear", 120, 70, 80, 30)

GUISetState(@SW_SHOW, $hGUI)

While 1
    Switch GUIGetMsg()
        Case $GUI_EVENT_CLOSE
            Exit

        Case $btnBrowse
            ; Browse for a file
            Local $filePath = FileOpenDialog("Select a file", @MyDocumentsDir, "Text Files (*.txt)|All Files (*.*)", $FD_FILEMUSTEXIST)
            If @error = 0 Then
                GUICtrlSetData($inputPath, $filePath)
            Else
                MsgBox(0, "Error", "No file selected.")
            EndIf

        Case $btnClear
            ; Clear the input
            GUICtrlSetData($inputPath, "")
    EndSwitch
WEnd


Question 5) Text Repeater

;; AutoIt script to repeatedly type a phrase into an open application

#include <MsgBoxConstants.au3>
#include <GUIConstantsEx.au3>

Global $phrase = InputBox("Auto Typer", "Enter the phrase to type:")
If @error Then Exit ; Exit if the user cancels

Global $repeat = InputBox("Auto Typer", "Enter the number of times to repeat:", "10")
If @error Then Exit ; Exit if the user cancels

If Not StringIsDigit($repeat) Or $repeat < 1 Then
    MsgBox($MB_ICONERROR, "Error", "Please enter a valid number.")
    Exit
EndIf

MsgBox($MB_ICONINFORMATION, "Instructions", "Switch to the target application and click OK. Typing will start in 3 seconds.")

Sleep(3000) ; Wait 3 seconds before starting

For $i = 1 To $repeat
    Send($phrase & "{ENTER}") ; Sends the phrase and presses Enter
    Sleep(500) ; Short delay to prevent overload
Next

MsgBox($MB_ICONINFORMATION, "Done", "Typing completed!")


Question 6) Automate Folder Creator


#include <MsgBoxConstants.au3>
#include <File.au3>

; Define the base directory where the folder will be created
Global $sBaseDir = "C:\MyFolders"

; Check if base directory exists, if not, create it
If Not FileExists($sBaseDir) Then
    DirCreate($sBaseDir)
EndIf

; Prompt the user to enter a folder name
Global $sFolderName = InputBox("Create Folder", "Enter the folder name:")

; Check if the user entered a name
If $sFolderName = "" Then
    MsgBox($MB_ICONWARNING, "Error", "No folder name entered. Exiting...")
    Exit
EndIf

; Construct the full path of the new folder
Global $sFolderPath = $sBaseDir & "\" & $sFolderName

; Check if the folder already exists
If FileExists($sFolderPath) Then
    MsgBox($MB_ICONEXCLAMATION, "Error", "Folder already exists!")
Else
    ; Create the folder
    DirCreate($sFolderPath)
    MsgBox($MB_ICONINFORMATION, "Success", "Folder '" & $sFolderName & "' created in " & $sBaseDir)
EndIf



Question 7) Automated Window Resizer

; Script to open Notepad, identify the window, and resize it

; Define the window title and dimensions
Local $windowTitle = "Untitled - Notepad"
Local $width = 600
Local $height = 400

; Run Notepad
Run("notepad.exe")

; Wait for the Notepad window to exist
If WinWait($windowTitle, "", 5) Then ; Wait up to 5 seconds for the window to appear
    ; Activate the window
    WinActivate($windowTitle)
    
    ; Wait for the window to become active
    WinWaitActive($windowTitle)
    
    ; Resize the window
    WinMove($windowTitle, "", Default, Default, $width, $height)
    
    ; Display a success message
    MsgBox(0, "Success", "Notepad has been opened and resized to " & $width & "x" & $height & ".")
Else
    ; If the window doesn't exist, show an error message
    MsgBox(16, "Error", "The Notepad window could not be opened or found.")
EndIf


Question 8) Simple Process Checker

Global $sProcessName = InputBox("Process Checker", "Enter the process name to check (e.g., notepad):")


If $sProcessName = "" Then
    MsgBox(48, "Process Checker", "No process name provided. Exiting...")
    Exit
EndIf


If StringRight($sProcessName, 4) <> ".exe" Then
    $sProcessName &= ".exe"
EndIf


If ProcessExists($sProcessName) Then
    MsgBox(64, "Process Checker", $sProcessName & " is already running.")
Else
    MsgBox(48, "Process Checker", $sProcessName & " is not running.")
EndIf


Question 9) Temporary File Cleaner


; Get the system temporary folder path
Local $tempFolder = @TempDir

; Count the number of files deleted
Local $deletedCount = 0

; Use FileFindFirstFile and FileFindNextFile to delete files
Local $search = FileFindFirstFile($tempFolder & "\*.*")

; Check if the folder is accessible
If $search = -1 Then
    MsgBox(16, "Temporary File Cleaner", "Failed to access the temp folder.")
    Exit
EndIf

; Loop through the files and delete them
While 1
    Local $file = FileFindNextFile($search)
    If @error Then ExitLoop

    If FileDelete($tempFolder & "\" & $file) Then
        $deletedCount += 1
    EndIf
WEnd

; Close the search handle
FileClose($search)

; Display a summary of the files deleted
MsgBox(64, "Temporary File Cleaner", $deletedCount & " files deleted.")


Question 10) Window Title Logger

; Include the Date UDF
#include <Date.au3>

; Path to the log file
Global $logFile = @ScriptDir & "\window_titles_log.txt"

; Initialize the log file (create or overwrite)
FileDelete($logFile)  ; Delete the file if it exists
FileWrite($logFile, "Window Title Log: " & @CRLF)

; Variable to store the previous window title
Global $lastWindowTitle = ""

While 1
    ; Get the title of the currently active window
    $currentWindowTitle = WinGetTitle("[ACTIVE]")
    
    ; If the window title has changed, log it
    If $currentWindowTitle <> $lastWindowTitle And $currentWindowTitle <> "" Then
        ; Log the new window title with a timestamp
        FileWrite($logFile, "[" & _NowCalc() & "] " & $currentWindowTitle & @CRLF)
        
        ; Update the last window title
        $lastWindowTitle = $currentWindowTitle
    EndIf
    
    ; Sleep for a short period to avoid high CPU usage
    Sleep(100)
WEnd


Question 11) Folder Size Calculator

#include <File.au3>

; Prompt user to select a folder
Global $folderPath = FileSelectFolder("Select a Folder", "")

; Check if the user selected a folder
If @error Then
    MsgBox(16, "Error", "No folder selected. Exiting script.")
    Exit
EndIf

; Function to calculate folder size
Func GetFolderSize($sFolder)
    Local $iSize = 0
    Local $aFileList = _FileListToArray($sFolder, "*", 1)
    
    If IsArray($aFileList) Then
        For $i = 1 To $aFileList[0]
            $iSize += FileGetSize($sFolder & "\" & $aFileList[$i])
        Next
    EndIf
    
    Local $aFolderList = _FileListToArray($sFolder, "*", 2)
    
    If IsArray($aFolderList) Then
        For $i = 1 To $aFolderList[0]
            $iSize += GetFolderSize($sFolder & "\" & $aFolderList[$i])
        Next
    EndIf
    
    Return $iSize
EndFunc

; Get the total size of the selected folder
Local $totalSize = GetFolderSize($folderPath)

; Convert size to human-readable format
Func ConvertSize($size)
    If $size < 1024 Then
        Return $size & " Bytes"
    ElseIf $size < 1048576 Then
        Return Round($size / 1024, 2) & " KB"
    ElseIf $size < 1073741824 Then
        Return Round($size / 1048576, 2) & " MB"
    Else
        Return Round($size / 1073741824, 2) & " GB"
    EndIf
EndFunc

; Display the result
MsgBox(64, "Folder Size", "The total size of the folder is: " & ConvertSize($totalSize))


Question 12) Window Focus Checker

; Script to log the titles of currently active windows to a text file

; Define the output log file
Global $logFile = @ScriptDir & "\ActiveWindowsLog.txt"

; Open the log file for appending
FileWrite($logFile, "--- Active Window Logger Started: " & @YEAR & "/" & @MON & "/" & @MDAY & " " & @HOUR & ":" & @MIN & ":" & @SEC & " ---" & @CRLF)

; Variable to store the last window title to avoid duplicate logging
Global $lastTitle = ""

While True
    ; Get the handle of the currently active window
    $activeWindowHandle = WinGetHandle("[ACTIVE]")

    ; Get the title of the currently active window
    $activeWindowTitle = WinGetTitle($activeWindowHandle)

    ; Check if the title has changed and is not empty
    If $activeWindowTitle <> $lastTitle And $activeWindowTitle <> "" Then
        ; Log the new window title with a timestamp
        FileWrite($logFile, @YEAR & "/" & @MON & "/" & @MDAY & " " & @HOUR & ":" & @MIN & ":" & @SEC & " - " & $activeWindowTitle & @CRLF)

        ; Update the last title
        $lastTitle = $activeWindowTitle
    EndIf

    ; Wait for a short interval before checking again
    Sleep(500)
WEnd


Question 13) Simple Text File Reader

; Script to open a specified .txt file, read its contents, and display them in a GUI

#include <GUIConstantsEx.au3> ; Include GUI constants

; Define the missing constants manually (if not already defined in your environment)
Global Const $ES_AUTOVSCROLL = 0x0040
Global Const $ES_MULTILINE = 0x0004
Global Const $WS_VSCROLL = 0x00200000

; Define the file to read
Global $filePath = FileOpenDialog("Select a Text File", @ScriptDir, "Text Files (*.txt)", 1)

; Check if the user selected a file
If @error Then
    MsgBox(16, "Error", "No file selected. Exiting script.")
    Exit
EndIf

; Open the file for reading
Local $fileHandle = FileOpen($filePath, 0)

; Check if the file was opened successfully
If $fileHandle = -1 Then
    MsgBox(16, "Error", "Failed to open the file. Exiting script.")
    Exit
EndIf

; Read the file contents
Local $fileContents = FileRead($fileHandle)

; Close the file
FileClose($fileHandle)

; Check if the file is empty
If $fileContents = "" Then
    MsgBox(48, "Info", "The file is empty.")
    Exit
EndIf

; Create a GUI to display the file contents
Local $gui = GUICreate("File Contents Viewer", 600, 400)
Local $edit = GUICtrlCreateEdit($fileContents, 10, 10, 580, 380, BitOR($ES_AUTOVSCROLL, $ES_MULTILINE, $WS_VSCROLL))
GUICtrlSetFont($edit, 10, 400, 0, "Consolas")

; Show the GUI
GUISetState(@SW_SHOW, $gui)

; Wait for the user to close the GUI
While True
    Switch GUIGetMsg()
        Case $GUI_EVENT_CLOSE
            ExitLoop
    EndSwitch
WEnd

; Clean up and exit
GUIDelete($gui)
Exit


Question 14) Simple Screenshot Tool  

#include <ScreenCapture.au3>
#include <Date.au3>
#include <MsgBoxConstants.au3>

; Set the folder where screenshots will be saved
Global $sSaveFolder = @DesktopDir & "\Screenshots"

; Ensure the folder exists
If Not FileExists($sSaveFolder) Then DirCreate($sSaveFolder)

; Set a hotkey (Ctrl + Shift + S) to capture a screenshot
HotKeySet("^+s", "CaptureScreenshot")

; Keep the script running
While True
    Sleep(100)  ; Prevents high CPU usage
WEnd

; Function to capture and save the screenshot
Func CaptureScreenshot()
    Local $sTimestamp = _Now()  ; Get current date-time for unique filenames
    Local $sFilePath = $sSaveFolder & "\Screenshot_" & StringReplace($sTimestamp, ":", "-") & ".png"

    ; Capture the screen and save as PNG
    _ScreenCapture_Capture($sFilePath)

    ; Notify the user
    MsgBox($MB_ICONINFORMATION, "Screenshot Taken", "Screenshot saved to: " & $sFilePath)
EndFunc

Question 15) Basic Countdown Timer

#include <GUIConstantsEx.au3>
#include <MsgBoxConstants.au3>

$gui = GUICreate("Timer", 250, 130)
GUICtrlCreateLabel("Time (Sec):", 10, 20, 100, 20)
$input = GUICtrlCreateInput("", 100, 20, 80, 20)
$btn = GUICtrlCreateButton("Start", 90, 60, 70, 30)
$lbl = GUICtrlCreateLabel("", 90, 100, 100, 20)

GUISetState(@SW_SHOW)

While 1
    Switch GUIGetMsg()
        Case $GUI_EVENT_CLOSE
            Exit
        Case $btn
            Local $t = Int(GUICtrlRead($input))
            If $t > 0 Then
                While $t > 0
                    GUICtrlSetData($lbl, $t & " sec left")
                    Sleep(1000)
                    $t -= 1  ; Corrected decrement
                WEnd
                MsgBox(64, "Time's Up!", "Countdown finished!")
                GUICtrlSetData($lbl, "")
            Else
                MsgBox(16, "Error", "Enter a valid number.")
            EndIf
    EndSwitch
WEnd



Question 16) Desktop Cleaner Helper

#include <File.au3>
Local $desktopPath = @DesktopDir
Local $shortcutFolder = $desktopPath & "\Shortcut"
If Not FileExists($shortcutFolder) Then
DirCreate($shortcutFolder)
EndIf
Local $files = _FileListToArray($desktopPath,"*.*", 1)
If Not @error Then
For $i = 1 To $files[0]
Local $sourceFile = $desktopPath & "\" & $files[$i]
Local $destinationFile = $shortcutFolder & "\" & $files[$i]
FileMove($sourceFile, $destinationFile, 9)
Next
Else
MsgBox(64,"Info","No files found on the desktop.")
EndIf
MsgBox(64,"Done","All files have been moved to the 'Shortcut' folder.")


Question 17) Custom Shutdown Timer

#include <GUIConstantsEx.au3>

$gui = GUICreate("Shutdown Timer", 300, 150)
GUICtrlCreateLabel("Enter time (minutes):", 20, 20, 250, 20)
$input = GUICtrlCreateInput("", 20, 50, 100, 20)
$btnSet = GUICtrlCreateButton("Set", 20, 90, 80, 30)
$btnCancel = GUICtrlCreateButton("Cancel", 120, 90, 80, 30)

GUISetState(@SW_SHOW)

While 1
    Switch GUIGetMsg()
        Case $GUI_EVENT_CLOSE
            ExitLoop
        Case $btnSet
            If StringIsInt(GUICtrlRead($input)) And GUICtrlRead($input) > 0 Then
                Run(@ComSpec & " /c shutdown -s -t " & GUICtrlRead($input) * 60, "", @SW_HIDE)
                MsgBox(64, "Shutdown", "Shutting down in " & GUICtrlRead($input) & " min(s).")
            Else
                MsgBox(16, "Error", "Enter a valid number.")
            EndIf
        Case $btnCancel
            Run(@ComSpec & " /c shutdown -a", "", @SW_HIDE)
            MsgBox(64, "Cancelled", "Shutdown canceled.")
    EndSwitch
WEnd

GUIDelete($gui)
Exit


Question 18) Random Password Generator

#include <MsgBoxConstants.au3>
#include <Array.au3>

Func GeneratePassword($length)
    Local $chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()-_=+[]{};:'"",.<>?/|`~"
    Local $password = StringMid($chars, Random(1, 52, 1), 1) & _ ; At least 1 letter
                     StringMid($chars, Random(53, 62, 1), 1) & _ ; At least 1 number
                     StringMid($chars, Random(63, StringLen($chars), 1), 1) ; At least 1 special

    For $i = 4 To $length
        $password &= StringMid($chars, Random(1, StringLen($chars), 1), 1)
    Next

    Local $array = StringSplit($password, "", 2)
    _ArrayShuffle($array)
    Return _ArrayToString($array, "")
EndFunc

Local $length = InputBox("Password Generator", "Enter password length (min 4):", "12")
If @error Or Not StringIsInt($length) Or $length < 4 Then Exit MsgBox($MB_ICONERROR, "Error", "Invalid input!")

MsgBox($MB_ICONINFORMATION, "Generated Password", "Your password: " & @CRLF & GeneratePassword(Int($length)))




Scripts:

19) Start Notepad and Calculator

RunWait('Calc')
RunWait('Notepad')
MsgBox(64,'Functions','I, Garv started a calculator and notepad')



20) Regex

#include <MsgBoxConstants.au3>
#include <StringConstants.au3>

Local $aResult = StringRegExp("This is a test example", '(test)', $STR_REGEXPARRAYMATCH)
If Not @error Then
	MsgBox($MB_OK, "SRE Example  Result", $aResult[0])
EndIf

$aResult = StringRegExp("This is a test example", '(te)(st)', $STR_REGEXPARRAYMATCH)
If Not @error Then
	MsgBox($MB_OK, "SRE Example 4 Result", $aResult[0] & "," & $aResult[1])
EndIf


21) Create own notepad

#include <GUIConstantsEx.au3>
#include <FileConstants.au3>

Global Const $ES_MULTILINE = 0x0004 ; Allows multiple lines in the text editor
Global Const $WS_VSCROLL = 0x00200000 ; Enables vertical scrolling in the edit box

; Create GUI
Global $gui = GUICreate("Notepad", 500, 400)
Global $edit = GUICtrlCreateEdit("", 10, 10, 480, 330, $ES_MULTILINE + $WS_VSCROLL)
Global $btnOpen = GUICtrlCreateButton("Open", 10, 350, 80, 30)
Global $btnSave = GUICtrlCreateButton("Save", 100, 350, 80, 30)

GUISetState(@SW_SHOW)

While 1
    Switch GUIGetMsg()
        Case $GUI_EVENT_CLOSE
            Exit
        Case $btnOpen
            Local $file = FileOpenDialog("Open File", @ScriptDir, "Text Files (*.txt)", $FD_FILEMUSTEXIST)
            If @error = 0 Then
                Local $hFile = FileOpen($file, $FO_READ)
                Local $content = FileRead($hFile)
                FileClose($hFile)
                GUICtrlSetData($edit, $content)
            EndIf
        Case $btnSave
            Local $file = FileSaveDialog("Save File", @ScriptDir, "Text Files (*.txt)", 2)
            If @error = 0 Then
                Local $hFile = FileOpen($file, $FO_OVERWRITE + $FO_CREATEPATH)
                FileWrite($hFile, GUICtrlRead($edit))
                FileClose($hFile)
            EndIf
    EndSwitch
WEnd
