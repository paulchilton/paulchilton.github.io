---
layout: post
title:  "Startup problem with LPC1347"
date:   2012-12-02
categories: electronics ARM
---

I was having difficulty getting my NXP LPC1347 to correctly run a program when it wasn't in debug mode connected to the programmer. It turns out that you need to explicitly set a pre-processor symbol to instruct the processor the start running your application when it boots, otherwise it just sits and idles.

In Keil this is found in the C/C++ tab of the project options. Enter 'STARTUP_FROM_RESET' in the preprocessor symbols box (put it in without the quotes).