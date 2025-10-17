PostgreSQL has a separate command-line tool that’s been available for decades and is included with any installation of PostgreSQL. Many long-term PostgreSQL users, developers, and administrators rely on psql to help them quickly connect to databases, examine the schema, and execute SQL queries.

Knowing how to install and use basic psql commands is an essential skill to have for anyone that will connect to PostgreSQL.
A Brief History of psql

To understand why PostgreSQL has its own command line utility and why it is so integrated into how developers work with PostgreSQL, we need to take a quick step into the time-travel machine and remind ourselves how PostgreSQL became PostgreSQL.
1970s-1985 	INGRES, the grandfather of PostgreSQL, developed and used a proprietary query language called QUEL. One of the terminal programs that was used to interact with INGRESS was called monitor
1985-1994 	POSTGRES (after INGRES) was started and developed. It maintained the QUEL query language now called POSTQUEL, and so the monitor application could continue to be used with POSTGRES
1995 	Postgres95 is released with a very liberal PostgreSQL License, based on POSTGRES source code, but QUEL is replaced with SQL! The monitor terminal application will no longer work, and psql was provided as the new terminal tool for interacting with Postgres95
1996-present 	PostgreSQL 6.0 was released under the guidance of the new PostgreSQL Global Development Group and amazing work hasn’t stopped since. With every release of PostgreSQL, additional features and improvements are made to the psql utility to provide more power to end users.
Why Should You Use psql

If you’re new to the PostgreSQL space, using a command line utility to connect to and query the database might feel outdated. As a former, long-time SQL Server developer I know the lack of a consistent, standardized GUI tool felt really frustrating at times.

After a while, however, I realized that it is often much easier to jump into a database with psql when I just need to connect, look at schemas, and run simple queries. Using the meta-commands becomes second nature for getting details about database objects. It’s like having a tool with hundreds of efficient shortcuts to quickly work with the database.

Even more powerful is that you can configure psql to return the SQL that it runs for each command along with the results. This can be a gateway into learning many of the underlying catalog tables in PostgreSQL that control every aspect of how the database is used.

And last, but certainly not least, is that many (many!) articles, tutorials, books, and videos that show you how to use PostgreSQL features will often demonstrate concepts using psql. Knowing how to connect with the tool and run basic meta-commands will quickly help you follow along.

TL;DR; – psql is a powerful tool to have in your PostgreSQL toolbelt. Knowing how to connect to a database and run basic commands will quickly pay dividends in your PostgreSQL work. In total, I think learning some of the basics is worth the time of investment.
Installing psql

The psql command line tool (CLI) is available on Linux, MacOS, and Windows. It comes pre-bundled with the PostgreSQL server installation package or it can be installed independently as a stand-alone CLI application.

Also, psql is versioned alongside PostgreSQL because the queries that it runs when you use meta-commands need to work with newer catalog schemas. There is a lot of effort taken to keep psql backwards compatible within the supported PostgreSQL versions, so a newer version of psql should work with at least the last five major releases.
Check For psql

Because psql comes bundled with the PostgreSQL server, there’s a possibility you already have it available on your computer if you’ve ever installed PostgreSQL. From a terminal or Windows command prompt, type the following:

$> psql --version
psql (PostgreSQL) 15.0 (Ubuntu 15.0-1.pgdg20.04+1)

If you see a response that lists the psql version, then you already have psqlinstalled. If the version is older, consider updating it.

A good rule of thumb is to try to have a psqlversion that matches the newest version of PostgreSQL you’re using across environments. For the basic commands we cover below there’s a high likelihood that any version you have will work, but keeping psqlupdated is a good practice to follow.
Install on Linux

All major Linux distributions should have a package for the postgresql-client. This can be used to only install the PostgreSQL tools apart from the server, including psql, pg_dump pg_restore, and others.

We’ll use Ubuntu as an example, but the package manager of your distribution should have identical (or very similar) package names to do the same thing. The commands below will install the latest version of the tools.

From a terminal prompt, run the following commands:

$> sudo apt-get update
 
$> sudo apt-get install postgresql-client

Once completed, verify installation by checking for the psql version as shown previously.
Install on Windows

On Windows, you have two options.

If you want to use psql through a Windows command prompt, then you need install PostgreSQL and all tools using the Windows installation package found on PostreSQL.org. Through the installation process, you can decide which parts of the server and tools to install.

On Windows 10 or greater, it’s possible to install the tools in a Windows Subsystem for Linux 2 (WSL2) environment and use the normal Linux package management to install the components as shown above. Again, depending on the distribution you’re using for the WSL host, the package commands may be slightly different.
Install on MacOS

There is a plethora of ways to install PostgreSQL on MacOS, many of which include the full server and associated tools.

The fastest method for getting PostgreSQL setup on MacOS is to use the Postgres.app. This provides a full installation of the PostgreSQL server that can be started and stopped at will, and the simple instructions on their homepage show you how to make sure the PostgreSQL CLI tools are in the path to use from the Terminal.

Alternatively, you can just install the PostgreSQL using Homebrew. Installing the libpq package only installs the tooling (not the server).

ryan@mba-laptop % brew doctor
ryan@mba-laptop % brew update
ryan@mba-laptop % brew install libpq

Because this isn’t the full PostgreSQL server, you’ll also need to setup the path correctly so that you can use psql from the Terminal. The easiest way to do that is to let Homebrew update the linking as follows.

ryan@mba-laptop % brew link --force libpq

Docker

Finally, I’d be remiss if I didn’t mention Docker as an alternative. Any of the official PostgreSQL containers can be used, through the interactive Docker shell, to connect to and use psql. Whether you’re attempting to connect to PostgreSQL on the Docker instance itself or remotely, the installed psqlapplication can be used (assuming networking is setup correctly).

As an example, the following commands could be used to download and start a PostgreSQL Docker container, connect to the running shell, and use psqlinside of the container.

ryan@redgate-laptop:~$ docker run --name pg15 -p 5432:5432 -e POSTGRES_PASSWORD=password -d postgres
 
ryan@redgate-laptop:~$ docker exec -it pg15 bash
 
root@80049acea1c0:/# psql -h localhost -U postgres

Connecting to PostgreSQL

Once you have psql installed there are two ways to specify the basic connection parameters for the target database. You will need the following information to connect to PostgreSQL.

    Hostname
    Port (5432 by default)
    Username
    Password
    Database name

Specifying Individual Parameters

With the above information, connect to the database using these parameters. If the PostgreSQL server is running on the default port of 5432, you can omit the -p switch and psql will attempt to connect to that port automatically.

psql -h [hostname] -p [port] -U [username] -d [database name]

If all the information is correct, you will be prompted for a password. When using the individual parameters for connecting to PostgreSQL, there is no method to supply the password on the command line. You will always be prompted, or you could specify it in the .pgpass file.
Using a PostgreSQL Connection URI

Alternatively, you can use the connection parameters to create a PostgreSQL connection URI to connect to the database.

psql postgresql://[username]:[password]@[hostname]:[port]/[database name]

With this form, you can specify the password in the connection string as long as it does not contain a semi-colon (;) or ampersand (@) as those interfere with the connection URI parsing. If the password is omitted, then PostgreSQL will prompt you.
Conclusion

The psql command-line application is the go-to tool for anyone working with PostgreSQL. With decades of development and hundreds of built-in meta-commands to help developers and administrators work with PostgreSQL quickly and efficiently, knowing how to install and use it to connect to databases is an essential skill for PostgreSQL users. As an added bonus, recent psql versions can be used with at least the most recent five major releases of PostgreSQL because of the tireless work by the community to maintain backwards compatibility.

Of course, knowing how to get it installed and connecting is only part of the puzzle. In our next article, we’ll give you a tour of how to use psql and the major meta-commands you should know to accelerate your development and management of PostgreSQL databases.