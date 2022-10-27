@echo off
@echo --:[ SOLUTION CLEANER ]:--
@echo Deleting all BIN, OBJ, PACKAGES folders...
for /d /r . %%d in (bin obj ) do @if exist "%%d" rmdir "%%d" /S /Q
@echo Deleting all packages folders...
for /d /r . %%d in (packages) do @if exist "%%d" rmdir "%%d" /S /Q 
@echo BIN, OBJ and PACKAGES folders successfully deleted :)
pause > nul
