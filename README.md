# GenerateArray
#include &lt;Array.au3> Global $ObjErr = ObjEvent("AutoIt.Error", "_ErrorHandler")  __Example1() Func __Example1()     Local $size = 100000     Local $array[$size]     GenerateArray($array)  #cs     Local $hTimer0 = TimerInit()     _ArraySort($array)     ConsoleWrite("for [ " &amp; $size &amp; " ] time = " &amp; TimerDiff($hTimer0) &amp; @CRLF)     ;for [ 10000 ] time = 501.983565939219     ;for [ 100000 ] time = 6123.68485518709 #ce  ;#cs     Local $hTimer1 = TimerInit()     Local $x_Result = __ArraySortJs($array, True)     ConsoleWrite("for [ " &amp; $size &amp; " ] time = " &amp; TimerDiff($hTimer1) &amp; @CRLF)     ;for [ 10000 ] time = 81.1680221733835 (if numeric = False)     ;for [ 10000 ] time = 113.865363887061 (if numeric = True)      ;for [ 100000 ] time = 978.238867813598 (if numeric = False)     ;for [ 100000 ] time = 1291.74552402816(if numeric = True)     ;_ArrayDisplay($x_Result) ;#ce EndFunc  ; #FUNCTION# ============================================================================= ; Name...........: __ArraySortJs ; ======================================================================================== Func __ArraySortJs($o_arry, $o_Isnumeric = False)     ;====     Local $o_Check = ($o_Isnumeric ? 'return JsArray.sort(NumericalSort)' : 'return JsArray.sort()' )     Local $o_CBlock = 'function GetArray(arr){' &amp; @CRLF &amp; _     'var oArray = new VBArray(arr)' &amp; @CRLF &amp; _     'return oArray.toArray()' &amp; @CRLF &amp; _     '}' &amp; @CRLF &amp; _     @CRLF &amp; _     'function NumericalSort(a, b){' &amp; @CRLF &amp; _     'return a - b;' &amp; @CRLF &amp; _     '}' &amp; @CRLF &amp; _     @CRLF &amp; _     'function ArraySort(arr){' &amp; @CRLF &amp; _     'var JsArray = GetArray(arr)' &amp; @CRLF &amp; _     $o_Check &amp; @CRLF &amp; _     '}'     ;====      ;====     Local $o_Obj = 0     $o_Obj = ObjCreate("ScriptControl")     $o_Obj.Language = "JScript"     $o_Obj.AddCode($o_CBlock)     Local $o_GData = $o_Obj.run("ArraySort" , $o_arry)     $o_Obj = 0     ;====      ;====     Local $oDict = ObjCreate("Scripting.Dictionary")     For $vKeys in $o_GData         $oDict.Item($vKeys)     Next     Local $oValue = $oDict.Keys()     $oDict = 0     ;====      Return $oValue EndFunc  Func _ErrorHandler($oError) EndFunc  Func GenerateArray(ByRef $a)   For $i = 0 To UBound($a) - 1     $a[$i] = Random(-1000000, +1000000, 1)   Next EndFunc   ;==>GenerateArray
