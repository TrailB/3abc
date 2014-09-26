//Executing a command without argument 

int executeCommand(char* command) {
	FILE *cmdIn;
	int status = 1;
	char readbuf[120];
	
	//Exception Handling
	if ((cmdIn = popen(command, "r")) == NULL) {
		printf("Error executing command\n");
		perror("Error executing command\n");
		longjmp(getinput, 1);
	}
	while (fgets(readbuf, 120, cmdIn))
	{
		status=0;
		printf("%s", readbuf);
	}
	//printf("\nFLAG is %d\n",flag);
	pclose(cmdIn);
	return status;
}



//Executing a command to redirect output to a file

int executeRedirectCommand(char* line) {
	FILE *pipein_fp, *redirect_fp;
	int redirect;
	char readbuf[128];
	int i, j;
	char* command1;
	char* command2;
	int pid, fd;
	int flag = 1;
	command1 = strtok(line, ">");
	command2 = strtok(NULL, ">");
	while (command2[i] == ' ') {
		j = 0;
		while (command2[j]) {
			command2[j] = command2[j + 1];
			j++;
		}
		i++;
	}
	redirect_fp = fopen(command2, "w");
	if ((pipein_fp = popen(command1, "r")) == NULL) {
		longjmp(getinput, 1);
		}
	while (fgets(readbuf, 80, pipein_fp)) {
	flag=0;
		fputs(readbuf, redirect_fp);
		fflush(redirect_fp);
	}
	fclose(redirect_fp);
	pclose(pipein_fp);
	return flag;
}

