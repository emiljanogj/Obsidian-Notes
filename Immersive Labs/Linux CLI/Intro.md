#Linux

***

#### Sticky bit

Sticky bit is a permission attribute set on directories to control how files are managed within them => Only the owner of a file or the root user can rename/delete the file. Useful in the `/tmp` directory for example, which is shared among multiple users => Users can create files but they can only modify their own files. To set the sticky bit we use the following command:
```
chmod +t /path/to/directory
```

- To remove the sticky bit we use the following command:
```
chmod -t /path/to/directory
```

#### wc
- Command Overview
`wc [file]` – count the number of lines, words, and characters in a file  
`wc -w [file]` – count the number words in a file  
`wc -l [file]` – count the number of lines in a file  
`wc -m [file]` – count the number characters in a file

#### Manipulating text

##### tr
- `tr [a] [b]` is used to replace every letter a with letter b.
- `tr [a] [b] < myfile.txt > newfile.txt` performs the substitution in file `myfile.txt` and saves it in `newfile.txt`.
- `tr` is aware of the following:
	- [:alnum:] - A-Za-z0-9
	- [:alpha:] - A-Za-z
	- [:digit:] - 0-9
	- [:punct:] - All punctuation characters

##### sed
`sed ‘s/pattern_to_find/pattern_to_replace/g’ filewitherror.txt`

*Note*: `tr` works on chars while `sed` works on strings.

#### Stream redirection
```bash
some-program 1> output 
some-program 2> error
some-program 1> output 2> error
some-program 1> output-and-error 2>&1
```

*Note*: The last command redirects the error output to the normal output => combining them both.

- To run a program and redirect the output to a given file we use the following command(assuming program is `program-error` and file is `error`) as follows:
```
program-error 2>error
```


#### find

`find /[directory-to-search] -name [filename]` – Search a directory for a file

`find /[directory-to-search] -perm [permissions as numerical value]` – Search a directory for everything that has particular user permissions

`find /[directory-to-search] -user [username]` – Search a directory for everything owned by a particular user


#### screen
Useful when we want to disconnect from a session and not lose the current progress.  To work with screens, we use the following commands:

![[Pasted image 20230623164516.png]]


#### awk

- General `awk` command is as follows:

```
$ awk options 'selection _criteria {action }' input-file > output-file
```

For example to print the 2nd and 3rd columns, we use the following command:

```
awk '{print $2 "\t" $3}' file.txt
```

- To print all lines that match a specific pattern, we use the following syntax:

```
awk '/variable_to_be_matched/ {print $0}' file.txt
```

For example, to print all the lines that contain the letter o, we use the following syntax:

```
awk '/o/ {print $0}' file.txt
```

- To print the number of lines that match a pattern, we use the following syntax:

```
awk '/a/{++cnt} END {print "Count = ", cnt}' file.txt
```

- To print the lines with a certain size:

```
awk 'length($0) > 20' file.txt
```

- To check for a specific string in a column, we use the following syntax:
```
awk '{ if($3 == "B6") print $0;}' file.txt
```

- For loop example with `awk`
```
awk 'BEGIN { for(i=1;i<=6;i++) print "square of", i, "is", i*i;}'
```

- **NR:** NR command keeps a current count of the number of input records. Remember that records are usually lines. Awk command performs the pattern/action statements once for each record in a file. 
- **NF:** NF command keeps a count of the number of fields within the current input record. 
- **FS:** FS command contains the field separator character which is used to divide fields on the input line. The default is “white space”, meaning space and tab characters. FS can be reassigned to another character (typically in BEGIN) to change the field separator. 
- **RS:** RS command stores the current record separator character. Since, by default, an input line is the input record, the default record separator character is a newline.
- **OFS:** OFS command stores the output field separator, which separates the fields when Awk prints them. The default is a blank space. Whenever print has several parameters separated with commas, it will print the value of OFS in between each parameter. 
- **ORS:** ORS command stores the output record separator, which separates the output lines when Awk prints them. The default is a newline character. print automatically outputs the contents of ORS at the end of whatever it is given to print.