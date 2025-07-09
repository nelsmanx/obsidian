
### curl

By default, the curl  command writes the content of the requested file to the standard output of the terminal.

#### Downloading a single file with curl

To physically download the file on your machine in the current directory, you can use the `-O`.  For example, this command will download and save the 20231003.txt  file in the current directory, under its original name:

```bash
$ curl -O http://example.com/logs/20231003.txt
```

#### Downloading and renaming files


