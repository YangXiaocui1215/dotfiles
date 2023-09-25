# Display color and default editor
export VISUAL=vim
export EDITOR="$VISUAL"
[ -f ~/.dotfiles/scripts/color.sh ] && source ~/.dotfiles/scripts/color.sh
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi






# Neovim settings
if [ -f $HOME/.dotfiles/nvim/nvim.appimage ]; then
    alias 'nvim'='$HOME/.dotfiles/nvim/nvim.appimage'
    alias 'vi'="nvim -u ~/.config/nvim/init.vim"
    alias 'vim'="nvim -u ~/.config/nvim/init.vim"
fi






# Python related
__conda_setup="$('${HOME}/miniconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "${HOME}/miniconda3/etc/profile.d/conda.sh" ]; then
        . "${HOME}/miniconda3/etc/profile.d/conda.sh"
    else
        export PATH="${HOME}/miniconda3/bin:$PATH"
    fi
fi
unset __conda_setup

if [ -f $HOME/.dotfiles/condarc ]; then
    export 'CONDARC'="$HOME/.dotfiles/condarc"
fi

# File "path/to/script.py", line 97, in method
File() {
    local input="$@"
    local file_path=$(echo "$1" | sed 's/,$//')
    local line_num=$(echo "$input" | awk -F'line ' '{print $2}' | awk -F',' '{print $1}')
    local cmd="$HOME/.dotfiles/nvim/nvim.appimage -u ~/.config/nvim/init.vim +$line_num $file_path -c 'normal zt'"
    echo "$cmd"
    eval $cmd
}

alias 'act'='conda activate'
alias 'deact'='conda deactivate'
alias 'cl'='conda info --envs'
alias 'p'='python'
alias 'py'='python'
alias 'ipdb'='python -m ipdb -c continue'
alias 'i'='python -m ipdb -c continue'
export PYTHONBREAKPOINT=ipdb.set_trace

pipin ()
{
    pip install -r ~/.dotfiles/scripts/requirements.txt --upgrade
}
alias 'smi'="py3smi -w 100"
alias 'lab'='jupyter lab --no-browser --allow-root --port ' # lab 8888
alias 'pp'='export PYTHONPATH=$(pwd):$PYTHONPATH'
alias 'site'='python -m site' # location of site packages
alias 'rig'='rig' # github history
alias 'icdiff'='icdiff' # difference in files
alias 'visidata'='visidata' # csv files
alias 'grip'='grip' # render readme
alias 'tb'='tensorboard --logdir . --port' # tb 8080






# Locate to directory
# autojump using "j"
[[ -s $HOME/.autojump/etc/profile.d/autojump.sh ]] && source $HOME/.autojump/etc/profile.d/autojump.sh
alias 'G'='cd ~/G'
alias 'D'='cd ~/.dotfiles'
alias '..'='cd ..'
alias '...'='cd ../..'
alias '....'='cd ../../..'
alias '.....'='cd ../../../..'






## Tmux
[ -f ~/.tmux.conf ] && tmux source-file ~/.tmux.conf \
&& tmux send-keys "conda activate $(tmux show-environment | grep ^pythonenv= | cut -d'=' -f2-)" C-m \
&& tmux send-keys "cd $(tmux show-environment | grep ^workdir= | cut -d'=' -f2-)" C-m
# export working environment for python and project folder
ee () {
  tmux set-environment pythonenv "$CONDA_DEFAULT_ENV"
  tmux set-environment workdir "$(pwd)"
  tmux show-environment | grep ^pythonenv
  tmux show-environment | grep ^workdir
}
alias 't'='tmux'
alias 'ta'='tmux a'






## iTerm2 integration
test -e "${HOME}/.iterm2_shell_integration.bash" && source "${HOME}/.iterm2_shell_integration.bash"
alias 'dl'='it2dl'
alias 'ul'='it2ul'
alias 'img'='imgcat  -H 1000px -s' # image files






# Daily commands
rm_all(){
    find . -name "$1" -type f
    find . -name "$1" -type f -delete
}

mkd ()
{
    mkdir -pv -- "$1" && cd -P -- "$1"
}
alias 'mkdir'='mkdir -pv'

gitls () {
  if git rev-parse --is-inside-work-tree > /dev/null 2>&1; then
    ls -d1 --color=always -CF $( (git ls-files && git ls-files --exclude-standard --others) | awk -F/ '{if(NF>1){print $1}else{print}}' | sort -u )
  else
    ls --color=always -CF
  fi
}
alias l='gitls'

alias a='alias'
alias ll='ls -FlaSh'
alias la='ls -A'
alias 'cp'='cp -iv'
alias 'mv'='mv -iv'
alias 'rmi'='rm -vI'
alias 'rm'='rm -v'
alias 'rmr'='rm -vr'
alias 'rm.'='current_dir=`pwd` && cd .. && rmr $current_dir && current_dir='
alias 'rmrf'='rm -rf'
alias 'cpr'='cp -r'
alias 'scpr'='scp -r'
alias 'g'='git'
alias 'gs'='g s' # git status -s in gitconfig
alias 'v'='vi'
alias 'b'='bash'
alias 'brc'='source ~/.bashrc'
alias 'zrc'='source ~/.zshrc'
alias 'bpf'='source ~/.bash_profile'
alias 'dush'='du -hxcs * 2>/dev/null | sort -hr'
alias 'dusha'='du -hxcs $(ls -A) 2>/dev/null | sort -hr'
alias 'drive'='~/.dotfiles/gdrive'