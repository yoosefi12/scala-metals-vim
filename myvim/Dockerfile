FROM ubuntu-jdk-scala:alpha-0.1


RUN apt-get update && apt-get -f install -y libncurses5-dev libgnome2-dev libgnomeui-dev \
	libgtk2.0-dev libatk1.0-dev libbonoboui2-dev \
	libcairo2-dev libx11-dev libxpm-dev libxt-dev python-dev \
	python3-dev ruby-dev lua5.1 liblua5.1-dev libperl-dev git

RUN cd ~ && git clone https://github.com/vim/vim.git
RUN cd ~/vim && ./configure --with-features=huge \
            --enable-multibyte \
            --enable-rubyinterp=yes \
            --enable-python3interp=yes \
            --with-python3-config-dir=$(python3-config --configdir) \
            --enable-perlinterp=yes \
            --enable-luainterp=yes \
            --enable-gui=gtk2 \
            --enable-cscope \
            --prefix=/usr/local \
	make VIMRUNTIMEDIR=/usr/local/share/vim/vim82


RUN cd ~/vim && make install

RUN update-alternatives --install /usr/bin/editor editor /usr/local/bin/vim 1
RUN update-alternatives --set editor /usr/local/bin/vim
RUN update-alternatives --install /usr/bin/vi vi /usr/local/bin/vim 1
RUN update-alternatives --set vi /usr/local/bin/vim

ENV JAVA_HOME /usr

#Installing nodejs and yarn (nodejs package manager)

RUN apt-get install -y curl software-properties-common \ 
	&& curl -sL https://deb.nodesource.com/setup_12.x | bash - \ 
	&& apt-get install -y nodejs

RUN curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
	&& echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \ 
	&& apt-get update && apt-get install -y yarn

#Installing vim-plug
RUN curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim


#Adding custom vimrc
COPY myvimrc /root/.vimrc

#Install vim plugins non-interactively from command line 
#RUN vim +'PlugInstall --sync' +qa
RUN vim +PlugInstall +qall

CMD ["/bin/bash"]
