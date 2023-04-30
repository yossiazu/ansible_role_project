
# project_ansible 

vars:
The var directory contains three encrypted variables, logs ,info and group_l, which contain the location of a specific file on the master computer. To access the values of these variables in your Ansible playbook or task, you would need to decrypt them using Ansible Vault.

group_vars: 

the group called g1 and  have some varibles:
ansible_user: The name of the user of the hosts in the group.
ansible_ssh_private_key_fil: The location of the ssh private key file.
syslog: The location of the syslog file in all of the hosts.
group: The location of the group file in all of the hosts.

the taskes:

task 1: this task removes the old files if they exist. It uses the file module to delete the files specified in the loop parameter. the files to be deleted are specified in the variables info, logs.

task 2: The second task using the basic ping module.

task 3: This task prints the IP address of the remote host. It uses the debug module to print the value of the ansible_default_ipv4.address variable, which is set by Ansible to the IP address of the host.

task 4: This task writes the computer name, operating system, and ip address of the host to a file. it uses the shell module to execute a shell command that appends the computer name, operating system, and ip address to the file specified in the info variable.

task 5: This task retrieves the log contents of the remote host. It uses the shell module to execute a shell command that searches for lines containing the words "ansible" and "april" (basicly looking for logs that ansible involved with him , and happend in april) in the file that contain in the group varibale syslog that point on /var/log/syslog file. The output of the command is stored in the log_contents register. If there are errors in executing the command, they are ignored. the notify parameter triggers the execution of the log handler if this task succeeds.

task 6: The copy the group file from all of the hosts task uses Ansible's fetch module to retrieve the group file from each host and save it to a specific folder on the master. The src option specifies the location of the file on each host, while the dest option specifies where the file should be saved on the master. The flat option is set to yes, meaning the file is saved with the same name and in the same location as it was on the remote host.

task 7: This task updates all the Debian hosts. It uses the apt module to update the computer. The when parameter ensures that this task is executed only on hosts with the ansible_os_family variable set to "Debian".

task 8: This task updates all the RedHat hosts. It uses the apt module to update the computer. The when parameter ensures that this task is executed only on hosts with the ansible_os_family variable set to "RedHat".


task 9: The reboot all hosts task uses Ansible's command module to run the reboot command on each host in the g1 group. The async option is set to 1 to send the command to each host asynchronously, and the delegate_to option is used to reboot each host one at a time. The loop option iterates over each host in the g1 group. Additionally, the poll option is set to 10 to check the status of the reboot command every 10 seconds, ensuring that each host completes the reboot process before moving on to the next one.

handler:

1: The purpose of this handler is to write the contents of the log register and the name of the computer to a file specified by the encrypted variable "logs". The handler is triggered whenever there is a change in the task "get the log contents". It uses the "shell" module to execute a shell command for writing to the file.
