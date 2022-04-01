# Local .terraform directories
**/.terraform/*

#В подкаталоге .terraform, расположенном в каталоге, содержащем файл .gitignore, игнорировать все файлы и директории


# .tfstate files
*.tfstate
*.tfstate.*
# игнорирование по типу файла. Файлы с расширением tfstate будут игнорироваться во всех директориях
# игнорировать файлы или директории, чье имя содержит .tfstate., например,  info.tfstate.c

# Crash log files
crash.log
crash.*.log

#игнорировать файлы crash.log, находящиеся в любом каталоге
#игнорировать файлы c любым названием, начинающимся на  crash. и заканчивающимся на .log, находящиеся в любом каталоге. Например, crash.a.log, crash.ast.log

# Exclude all .tfvars files, which are likely to contain sensitive data, such as
# password, private keys, and other secrets. These should not be part of version 
# control as they are data points which are potentially sensitive and subject 
# to change depending on the environment.
*.tfvars
*.tfvars.json

# игнорирование по типу файла. Файлы с расширением tfvars будут игнорироваться во всех директориях
# Файлы с  tfvars.json в конце будут игнорироваться во всех директориях, например, terraform.tfvars.json

# Ignore override files as they are usually used to override resources locally and so
# are not checked in
override.tf
override.tf.json
*_override.tf
*_override.tf.json

#игнорировать файлы override.tf, находящиеся в любом каталоге
#игнорировать файлы override.tf.json, находящиеся в любом каталоге
#игнорировать файлы, заканчивающиеся на   _override.tf и _override.tf.json, находящиеся в любом каталоге
#например temp_override.tf.json

# Include override files you do wish to add to version control using negated pattern
# !example_override.tf

# Include tfplan files to ignore the plan output of command: terraform plan -out=tfplan
# example: *tfplan*

# Ignore CLI configuration files
.terraformrc
terraform.rc
#игнорирование файлов, находящихся в любом каталоге, например tmp/terraform.rc или terraform.rc или .terraformrc

It's usually very cold here in winter


