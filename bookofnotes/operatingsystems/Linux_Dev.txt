# C

## pipes & fork

```c
#include <unistd.h>

int main(int argc, char **argv)
{
    int pipes[2];

    if ( pipe(pipes) == -1 )
    {
        return ERROR;
    }

    pid_t child_pid = fork();
    if ( child_pid < 0 )
    {
        return ERROR;
    }
    else if ( child_pid == 0 ) // we are child
    {
        // we will write only
        close(pipes[0]);

        // this child standard output will be written to the pipe
        dup2(pipes[1], STDOUT_FILENO);

        // we close the original pipe, but things are aready set up
        close(pipes[1]);

        // now you can just printf something, or lets execute other program
        execl("/bin/ls", "ls", "-l", "/home", 0);

        // we shouldn't be here, but if we are, the exec failed, check errno
        return -1;
    }
    
    // here we are the parent

    // reading from pipe using streams

    // we won't write
    close(pipes[1]);

    // for example read lines
    FILE * f = fdopen(pipes[0], "r");
    if ( !f )
        return ERROR; 
    
    char buffer[YOUR_DESIRED_SIZE];
    while ( fgets(buffer, sizeof(buffer), f) )
    {
        printf("%s", buffer); // it includes new line
    }

    fclose(f);

    // getting the exit code of child
    int exit_code = 0;
    waitpid(child_pid, &exit_code, 0);

    // do as you wish with the exit code

    return 0;
}
```

## shared memory

```c
#include <stdlib.h>
#include <errno.h>
#include <string.h>
#include <sys/mman.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

int shared_fd = shm_open("/aname", O_RDWR | O_CREAT | O_EXCL, S_IRUSR | S_IWUSR);
if ( shared_fd == -1 )
{
    return errno;
}

if ( ftruncate(shared_fd, desired_size) == -1 )
{
    int err = errno;
    close(shared_fd);
    shm_unlink("/aname");
    return err;
}

void * shared_buffer = mmap(0, desired_size, PROT_READ | PROT_WRITE, MAP_SHARED, shared_fd, 0);
if ( shared_buffer == MAP_FAILED)
{
    int err = errno;
    close(shared_fd);
    shm_unlink("/aname");
    return err;
}

// after this point we can close the file descriptor
close(shared_fd);

// ... later if you don't want to use the shared memory
// mummap is automatic when program exits.
munmap(shared_fd, desired_size);

// if you create the memory, you should delete too
shm_unlink(memory_name);

// Mapping from the other side (not creating)
#include <sys/types.h>

int shared_fd = shm_open("/aname", O_RDWR, 0);
if ( shared_fd == -1 )
{
    return errno;
}

struct stat memstat;
if ( fstat(shared_fd, &memstat) == -1 )
{
    int err = errno;
    close(shared_fd);
    return err;
}

// for example, read only
mmap(0, memstat.st_size, PROT_READ , MAP_SHARED, shared_fd, 0);

// close fd here



```