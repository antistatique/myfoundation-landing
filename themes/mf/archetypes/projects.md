+++
title = "{{ replace .Name "-" " " | title }}"
date = {{ .Date }}
description = ""
project_logo = ""
project_link = ""
project_category = ""
project_date = "{{ time.Now.Format "2006-01" }}"
draft = false
+++