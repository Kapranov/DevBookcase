V ?= @
LOCALHOST := 'localhost'
PORT := '3000'
IP := '127.0.0.1'
RUBYSERVICE := $(shell pgrep ruby)
NGINXSERVICE := $(shell pgrep nginx)
UNICORNSERVICE := $(shell pgrep unicorn)
THINSERVICE := $(shell pgrep thin)
VERSION := '0.0.1'
LOCALSERVER := 'http://127.0.0.1:3000'
ENV_DEVELOPMENT := 'development'
ENV_PRODUCTION := 'production'
ENV_TEST := 'test'

default:
	$(V)echo Please use \'make help\' or \'make ..any_parameters..\'

help:
	$(V)clear
	$(V)echo "\n\n\t Manual Makefile to start of the project:\n\n make bundle    - installing libraries\n make clean     - clean temporary files\n"

kill_ruby:
	$(V)echo "\nChecking to see if RUBY process exists:\n"
	$(V)if [ "$(RUBYSERVICE)" ]; then killall ruby && echo "Running Ruby Service Killed"; else echo "No Running Ruby Service!"; fi

processes:
	$(V)ps aux | grep 'ruby' | awk '{print $2}' | xargs kill -9

nginx: kill_ruby
	$(V)echo "\nChecking to see if Nginx process exists:\n"
	$(V)if [ "$(NGINXSERVICE)" ]; then service nginx stop && echo "Running Nginx Service Killed"; else echo "No Running Nginx Service!"; fi
	$(V)echo "\nChecking to see if Unicorn process exists:\n"
	$(V)if [ "$(UNICORNSERVICE)" ]; then ps aux | grep 'unicorn' | awk '{print $2}' | xargs kill -9 && echo "Running Unicorn Service Killed"; else echo "No Running Unicorn Service!"; fi

bundle:
	$(V)bundle

clean:
	$(V)rm -rf ./.bundle
	$(V)rm -rf ./tmp/*
	$(V)rm -rf ./public/assets/
	$(V)rm -rf ./Gemfile.lock
	$(V)bundle exec rake tmp:clear
	$(V)bundle exec rake log:clear

start: kill_ruby nginx clean bundle
	$(V)echo "\n\n\t Start $(ENV_DEVELOPMENT) Enviroment on $(LOCALSERVER)\n\n"
	$(V)rails server
