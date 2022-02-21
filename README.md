***опишите своими словами какие файлы будут игнорироваться в будущем благодаря добавленному .gitignore***

[^-] \*\*/.terraform/\*

Игнорирование всех файлов в директории /.terraform/

[^-] \*.tfstate

Игнорирование файлов с расширением \*.tfstate во всем репозитории

[^-] \*.tfstate.\*

Игнорирование файлов где встречается \*.tfstate.\*, например xxx.tfstate.kkkk

[^-] crash.log

Игнорирование файла crash.log в любой директории репозитория

[^-] \*.tfvars, \*tfvars.json, \*override.tf, \*override.tf.json

Аналогично с \*.tfstate

[^-] override.tf, override.tf.json, .terraformrc, terraform.rc

Аналогично с crash.log
