---
layout: post
title: "mechanize useage"
description: ""
category: Web
tags: [Rails]
---

[Mechanize (http://mechanize.rubyforge.org/GUIDE_rdoc.html#label-Getting+Started+With+Mechanize)](http://mechanize.rubyforge.org/GUIDE_rdoc.html#label-Getting+Started+With+Mechanize)

### example

**baidu search**:

    require  "mechanize"
    agent = Mechanize.new
    page = agent.get("http://www.baidu.com")
    result_page = page.form_with(name: "f") do |form|
    form["wd"] = "ruby"
    end.submit

**login cms**:

    agent = Mechanize.new
    page = agent.get("http://cms.stage.lanshizi.com/admin/login")
    login_form = page.form_with(action: "/admin/login") do |form|
      form["admin_user[username]"] = "sa"
      form["admin_user[password]"] = "sa"
    end
    cms_page = login_form.submit
    if message = cms_page.at(".flash_alert")
      p message.text
    else
      p cms_page
    end

**captcha 破解**

 * 破解破解验证码 captcha
 * 破解同个客户端获取captcha图片并且保存
 * 破解RTesseract 解释图片

  **code**:

    @agent.get(@captcha_url).save("captcha.jpg")
    @app_list_page = @agent.get(@login_url).form_with(id: "loginForm") do |f|
    f.action = "/accounts/signin/"
      f["username"] = @username
      f["password"] = @password
      f["captcha"] = RTesseract.new("captcha.jpg").to_s.strip
    end.click_button
