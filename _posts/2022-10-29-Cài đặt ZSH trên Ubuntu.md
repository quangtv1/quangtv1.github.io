---
layout: post
title: "Cài đặt ZSH trên Ubuntu"
subtitle: "Khởi tạo cấu hình  cơ bản"
date: 2022-10-29 00:45:13 -0400
background: '/img/posts/01.jpg'
---

### Cài đặt ZSH trên Ubuntu
------------------
```bash
sudo apt update

sudo apt install -y zsh

which zsh

sudo chsh -s /usr/bin/zsh

sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc

vi ~/.zshrc
> plugins=(colored-man-pages history jsontools zsh-autosuggestions zsh-syntax-highlighting git)

source ~/.zshrc
```


