---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date | time.Format ":date_medium" }}
description: "A Description of the post"
draft: true
author: "Derek Cash-Peterson"
cover: "images/cover.jpg"
tags: ["tag1"]
theme: "light"
---