//Executing a command without argument 

int executeCommand(char* command) {
	FILE *cmdIn;
	int status = 1;
	char readbuf[120];
	
	//Exception Handling
	if ((cmdIn = popen(command, "r")) == NULL) {
		printf("Error executing command\n");
		perror("Error executing command\n");
		return(1)
	}
	while (fgets(readbuf, 120, cmdIn))
	{
		//Writing content to stdout
		status=0;
		puts(readbuf);
	}
	
	pclose(cmdIn);
	return status;
}



//Executing a command to redirect output to a file

int RedirectOutputCommand(char* InputCommand) {
	FILE *inputfilep, *redirectedfilep;
	int status = 1;  //Setting an exit status
	int a, b, fd, pid;
	char* command1;
	char* command2;
	char readbuf[120]
	int redirect;
	
	//Breaking the input command into a series of tokens using delimiter  >
	
	command1 = strtok(InputCommand, ">"); //Getting  the first token  
	
	command2 = strtok(NULL, ">"); 
	while (command2[a] == ' ') {
		b = 0;
		while (command2[b]) {
			command2[b] = command2[b + 1];
			b++;
		}
		a++;
	}
	redirectedfp = fopen(command2, "w");
	
	//Exception Handling
	if ((inputfilep = popen(command1, "r")) == NULL) {
		return(1);  //Setting exit status indicating an error
		}
		
	//Reading file	
	while (fgets(readbuf, 120, inputfilep)) {
		status=0;  //Setting an exit status
		fputs(readbuf, redirectedfp); //Writing strings from inputfile to the stream of file opened for command2
		fflush(redirectedfp); //Flushing output buffer of the stream
	}
	fclose(redirectedfp);  //Closing redirected file pointer
	pclose(inputfilep);  //Closing input file pointer
	return status;
}

