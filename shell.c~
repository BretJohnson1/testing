#include "shell.h"

void no_more_betty(void);

/**
 * main - Entry point for ghost
 * @argc: arg count
 * @argv: array of input arg strings
 * @env: array of env strings
 * Return: Always 0. Exit status differs.
 */
int main(int argc, char **argv, char **env)
{
	char *line = NULL, *path = NULL;
	size_t nread = 0;
	char *args[20], *paths[20];
	int ret_val = 0;
	int i;

	(void) argc;
	(void) argv;
	while (1)
	{
		i = 0;
		while (env[i])
		{
			if (_strncmp(env[i], "PATH=", 5) == 0)
			{
				path = _strdup((env[i] + 5));
				break;
			}
			i++;
		}
		tokenize_string(path, ":", paths);
		if (isatty(STDIN_FILENO))
			write(STDOUT_FILENO, "$aylerMoon: ", 12);
		if (getline(&line, &nread, stdin) == -1)
		{
			free(line);
			free(path);
			exit(EXIT_SUCCESS);
		}
		tokenize_string(line, " \n\t", args);

		if (!args[0])
		{
			free(path);
			continue;
		}
		if (_strcmp(args[0], "exit") == 0)
		{
			free(line);
			free(path);
			exit(EXIT_SUCCESS);
		}
		if (_strcmp(args[0], "env") == 0)
		{
			for (i = 0 ; env[i] != NULL ; i++)
			{
				write(STDOUT_FILENO, env[i], _strlen(env[i]));
				write(STDOUT_FILENO, "\n", 1);
			}
			free(path);
			continue;
		}

		if (access(args[0], X_OK) == 0)
			make_a_baby(path, args[0], args);
		else
			ret_val = check_path(path, paths, args);

	}
	return (ret_val);
}
/**
 * tokenize_string - tokenize a string
 * @line: string to tokenize
 * @delims: delimiter string
 * @tokens: array to save tokens
 * Return: void
 */
void tokenize_string(char *line, char *delims, char **tokens)
{
	char *path_token = strtok(line, delims);
	int i = 0;

	while (path_token != NULL)
	{
		tokens[i] = path_token;
		i++;
		path_token = strtok(NULL, delims);
	}
	tokens[i] = NULL;
}
/**
* check_path - checks if command is in path
* @path: path string
* @patharr: array of path strings
* @args: array of input arg strings
* Return: 0 on success, 1 on failure
*/
int check_path(char *path, char **patharr, char **args)
{
	int i = 0;
	char *comp_path = NULL;
	struct stat x;

	while (patharr[i] != NULL)
	{
		comp_path = malloc(_strlen(args[0]) + _strlen(patharr[i]) + 2);
		_strcpy(comp_path, patharr[i]);
		_strcat(comp_path, "/");
		_strcat(comp_path, args[0]);
		if (stat(comp_path, &x) == 0)
		{
			make_a_baby(path, comp_path, args);
			free(comp_path);
			return (0);
		}
		free(comp_path);
		i++;
	}
	return (127);
}


/**
 * make_a_baby - creates a child process
 * @stdpath: path string
 * @call_path: path to executable
 * @str_arr: array of tokens
 * Return: status of child process
 */

int make_a_baby(char *stdpath, char *call_path, char **str_arr)
{
	pid_t cop;
	pid_t sig;
	int status = 0;

	free(stdpath);
	cop = fork();
	if (cop == 0)
	{
		if (execve(call_path, str_arr, NULL) == -1)
			exit(EXIT_FAILURE);
	}
	else if (cop < 0)
	{
		exit(EXIT_FAILURE);
	}
	else
	{
		do {
			sig = waitpid(cop, &status, WUNTRACED);
		} while (!WIFEXITED(status) && !WIFSIGNALED(status));
	}
	(void) sig;
	return (status);
}
/**
 * *_strdup - ret ptr to newly allocated mem space
 * @str: string
 * Return: NULL or ptr to dupe string
 */
char *_strdup(char *str)
{
	int i = 0;
	int n = 0;
	char *ar;

	if (str == NULL)
	{
		return (NULL);
	}
	while (str[i] != '\0')
	{
		i++;
		n++;
	}
	n++;

	ar = malloc(n * sizeof(char));
	if (ar == NULL)
	{
		return (NULL);
	}

	for (i = 0 ; i < n ; i++)
	{
		ar[i] = str[i];
	}

	return (ar);
}
