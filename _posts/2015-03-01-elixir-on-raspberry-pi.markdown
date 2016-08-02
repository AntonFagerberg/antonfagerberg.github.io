---
  layout: post
  title: "Install Elixir on a Raspberry Pi"
  categories: blog
  alias: /texts/elixir-on-raspberry-pi/
---

This is a tutorial of how to install Elixir (from source) on a Raspberry Pi using the distribution Raspbian.

The first thing we need to do is to add the Erlang Solutions repository to our system. Open the file /etc/apt/sources.list with your favorite editor and add the following line to it:

```text
deb http://packages.erlang-solutions.com/debian wheezy contrib
```

After saving the file, we need to add the repository's public key to our systems keychain:

```text
wget http://packages.erlang-solutions.com/debian/erlang_solutions.asc
sudo apt-key add erlang_solutions.asc
```

After this is done, we can remove the file erlang_solutions.asc if we want to.

Now we're ready to update our systems repositories:

```text
sudo apt-get update
```

After that has been done, we can begin install the needed packages:

```text
sudo apt-get install git erlang
```

Git is a version control system which we'll use for fetching Elixir. Erlang-mini is a tiny Erlang package for small devices (no GUI related dependencies and so on). Erlang-eunit is not really required to build Elixir itself but it will fail the unit-tests after the compilation is done which means that we can't know for sure that the system is working properly.

After the packages has been installed, we're ready to get Elixir from GitHub:

```text
git clone https://github.com/elixir-lang/elixir.git
cd elixir
```

Now, optionally, we can checkout a specific version of Elixir if we want to:

```text
git checkout tags/v1.0.3
```

We're now ready to compile Elixir:

```text
make clean test
```

This will take several minutes so go get yourself a coffee or beverage of choice while the Raspberry Pi is chewing away.

After the compilation has completed and all unit-tests has passed, you've got yourself Elixir ready to use!

Now you probably want to add the bin folder for Elixir to your path. This can be done temporarily by executing the following command in the terminal - or permanently by adding it to the file .profile in your home folder reloading the shell.

```text
export PATH="$PATH:/path/to/elixir/bin"
```

Be sure to change the path to the appropriate path where you cloned and compiled Elixir.

Now you're all set - happy hacking!
