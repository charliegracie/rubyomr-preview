# Introducing the "Ruby + OMR Technology Preview"

The OMR team has put this technology preview together to showcase how OMR runtime technology might be integrated into the Ruby VM.
We are releasing this technology preview for a couple of reasons:

1. So that people can try out the OMR technology in Ruby
2. To get feedback on how OMR technology has been integrated into Ruby

Our sincerest hope is that this preview helps us to work with the existing Ruby community to integrate
those parts of OMR that the Ruby community finds beneficial.

Platform support:
* Ubuntu 15.10 Linux on x86, 64-bit
* ClefOS 7.1 Linux on Z, 64-bit
* Ubuntu 14.04 Linux on OpenPower, 64-bit

Our lawyers are keen to tell you that any reference to the Invoice in the License means that you are authorised to
use one copy of the downloaded image.

If you can't wait to get started, you can go directly to the <a href="#quickstartguide">Quick Start Guide</a>

Otherwise, you may be wondering…


# What is “OMR” ?

The OMR project is an [open source project](https://github.com/eclipse/omr), initiated by IBM, to develop reusable and easily
consumable core components for building all kinds of language runtimes, from Java to Ruby to Smalltalk
and beyond. The initial components come originally from the IBM J9 Java Virtual Machine, an enterprise
class JVM implementation representing hundreds of person years of development creating scalable, high
performance runtime technology. It has been the core runtime for the IBM SDK, Java Technology Edition,
since 2005.

IBM has been distilling the core technology from this JVM to create components that can be used to
build all kinds of different language runtimes, free from the influence of Java semantics. It hasn’t
been easy, and we’re still not quite done, but soon these components will become available in an
open source project, possibly under the Eclipse foundation.

We are calling this technology “OMR”. You may wonder why IBM is opening up its runtime technology to
the OMR project. IBM has deep interest in creating truly vibrant cloud and Platform-as-a-Service
(PaaS) environments, where universal access to all sorts of language runtimes must become the new
normal. This polyglot world means that infrastructure, tools, hardware, and software all need to be
able to seamlessly work together with a consistent user experience so that developers can choose the
best language for the job.
With every runtime implemented differently, the path to this degree of seamlessness will be really
hard and take a really long time. Plus every new language will also need to go through that same
arduous ramp-up process which gets worse as mature runtimes become even more capable. With common
runtime components, everyone (including IBM) can all better leverage our efforts to make runtimes
better, faster, more capable, and more integrated to accelerate bringing not just the promise of
cloud computing but also the reality that developers should expect from a cloud computing environment.
We want to make it easier to reach new heights, not just for existing language runtimes but even more
importantly for the amazing new languages that developers have yet to imagine.

If you’d like to hear more about the OMR project, see [the Github page](https://github.com/eclipse/omr)
for the project which serves as its home page.  

# So what’s in this Ruby + OMR preview?

To make sure the OMR technology really could work in runtimes other than Java, the OMR team has
been working to create several internal proof points to use the technology with different language
runtimes (like Ruby!). This technology preview release represents the current state of our Ruby proof
point, with new GC, JIT compiler, and method profiling capabilities. It's not a toy: we've got it
running Rails applications. We cannot claim it is ready for use in production, but we think it's good
enough to be able to meet the goals outlined at the beginning of this document.

We have two talks specifically about our Ruby+OMR proof point at Ruby Kaigi 2015. Matthew's talk is
now on slideshare; When Robert's and Craig's slides go online we'll update the link below:

* Matthew Gaudet at Ruby Kaigi:
  [Experiments in sharing Java VM technology with CRuby](http://www.slideshare.net/MatthewGaudet/experiments-in-sharing-java-vm-technology-with-cruby)

* Robert Young and Craig Lehmann at Ruby Kaigi:
  [It's dangerous to GC alone. Take this!](http://www.slideshare.net/craiglehmann/the-omr-gc-talk-ruby-kaigi-2015)

# More about the preview

The preview is based on Ruby 2.2.3, which was the most recently available Ruby version when we
released it in mid December 2015. We felt that a docker image would be the easiest and most reliable
way for us to get the technology into people's hands with a minimum of fussing over platform specifics.
The image has a preinstalled Ruby 2.2.3 with built-in OMR technology. Also included is a monitoring agent
which can be used with IBM Health Centre to visualize Ruby method profiles and garbage collection
performance while your Ruby application is running.

Each docker image includes a git repo located at /home/rubyomr/ruby.  This repo contains the base
Ruby 2.2.2 (notice the last 2 isn't 3!) as well as a branch called "rubyomr-preview" with a single
commit that includes both the changes needed to integrate OMR into Ruby as well as (sorry) the changes
that came in Ruby 2.2.3 . Unfortunately, due to some sad accidents, we did not do a careful enough job updating
our internal repository to 2.2.3 and so 2.2.3 changes and OMR changes mingled. We discovered that issue
so late that we did not have time to untangle the commit history in time to deliver this source code for
Ruby Kaigi 2015. Instead, we merged those changes together. Hopefully, the mix won't be too hard to
understand because virtually all the OMR related changes are protected by macros with "OMR" in them.
Those macros enable us to use configure options to select whether or not to include major components like
the OMR JIT compiler or the OMR Garbage Collector. The git repo in the docker image has the
rubyomr-preview branch checked out.

The OMR technology itself is included only as part of the prebuilt binaries which you can access by
simply running ruby (/usr/local/bin/ruby). Unfortunately, whole of the OMR project is not quite available in the open
(sorry! we're working on it *really* hard).

We plan to  update the image to include the OMR source code along with more of
the glue code that connects the Ruby VM to OMR.

Not all of the OMR technologies are active by default in this version of Ruby.  Environment variables
activate those technologies that are not on by default, like the method profiling support when you
connect to ruby with IBM Health Center (also included in the docker image: see the User's Guide!) or to
turn the JIT compiler on. For example, to activate the JIT compiler technology, you'll need to set
`OMR_JIT_OPTIONS="-Xjit"` which turns on the JIT where it will compile methods that are invoked more than
1000 times. `OMR_JIT_OPTIONS="-Xjit:count=N"` adjusts the invocation count before JIT compilation so that
you can play around a bit.

For full details on how to activate each OMR component and the various configuration options available
in this technology preview, please see the
[User’s Guide in the Wiki](https://github.com/rubyomr-preview/rubyomr-preview/wiki) !

<a name="quickstartguide"></a>
# Quick Start Guide

The following files are in this project:

* This README.md file
* A LICENSE directory describing in excruciating detail just how little you can
  rely on in this technology preview

You can also find the [User's Guide in our wiki](https://github.com/rubyomr-preview/rubyomr-preview/wiki).

To start using the Ruby+OMR Technology Preview, please follow the directions for the platform you're using: Linux on X86,
Linux on Z, or Linux on OpenPOWER.

## Linux on X86, 64-bit

1. If you do not already have docker installed, follow these directions to get started:

   [Installing Docker For Linux on x86](http://docs.docker.com/engine/installation/)

2. Download the rubyomrpreview docker image from Box.com with the command:

        $ wget https://ibm.box.com/shared/static/1vlbbf3w6g8usthf91p40m3t1x8hd5hs.tgz -O rubyomrpreview-x86_64.tgz

3. Load the docker image locally:

        $ docker load -i rubyomrpreview-x86_64.tgz

4. Run the docker image (you can omit the -p 1883:1883 if you won't be using Health Centre or if you won't
   be running the message broker--see the [User's Guide](https://github.com/rubyomr-preview/rubyomr-preview/wiki)-- inside the container):

        $ docker run -p 1883:1883 -it rubyomrpreview/rubyomrpreview /bin/bash

5. Verify you can successfully run Ruby+OMR Technology Preview:

        root@d2ae8cf89313:/# ruby --version
        ruby 2.2.3p97 (OMR Preview r1)(2015-04-14) [x86_64-linux]

6. Play to your heart's content!

## Linux on Z, 64-bit

1. If you do not already have docker installed, follow these directions to get started:

   [Installing Docker For Linux on Z](http://www.ibm.com/developerworks/linux/linux390/docker.html)

2. Download the rubyomrpreview docker image from Box.com with the command:

        $ wget https://ibm.box.com/shared/static/1bzikt7fmfdejpvp4zqglnss64tcwls2.tgz -O rubyomrpreview-s390x.tgz

3. Load the docker image locally:

        $ docker load -i rubyomrpreview-s390x.tgz

4. Run the docker image (you can omit the -p 1883:1883 if you won't be using Health Centre or if you won't
   be running the message broker--see the [User's Guide](https://github.com/rubyomr-preview/rubyomr-preview/wiki)-- inside the container):

        $ docker run -p 1883:1883 -it rubyomrpreview/rubyomrpreview /bin/bash

5. Verify you can successfully run Ruby+OMR Technology Preview:

        bash-4.2# ruby --version
        ruby 2.2.3p97 (OMR Preview r1)(2015-04-14) [s390x-linux]

6. Play to your heart's content!

## Linux on OpenPOWER, 64-bit

   [Installing Docker for Linux on OpenPOWER](https://www.ibm.com/developerworks/library/l-docker/)

2. Download the rubyomrpreview docker image from Box.com with the command:

        $ wget https://ibm.box.com/shared/static/hdqgfvvnnq1idf6qvlsv9r0rw5dzwbhc.tgz -O rubyomrpreview-powerpc64le.tgz

3. Load the docker image locally:

        $ docker load -i rubyomrpreview-powerpc64le.tgz

4. Run the docker image (you can omit the -p 1883:1883 if you won't be using Health Centre or if you won't
   be running the message broker--see the [User's Guide](https://github.com/rubyomr-preview/rubyomr-preview/wiki)-- inside the container):

        $ docker run -p 1883:1883 -it rubyomrpreview/rubyomrpreview /bin/bash

5. Verify you can successfully run Ruby+OMR Technology Preview:

        root@7305793b79e9:/# ruby --version
        ruby 2.2.3p97 (OMR Preview r1) (2015-04-14) [powerpc64le-linux]

6. Play to your heart's content!

To see how to use the various OMR technologies, please look for Tracing,
Garbage Collector, and Just In Time (JIT) compiler sections in the
[User’s Guide in the Wiki](https://github.com/rubyomr-preview/rubyomr-preview/wiki) !

# We would love to hear your feedback!

As we said above, we’re making this technology preview openly available so that everyone can
try it out and let us know what's good and what's not so good.  We welcome all feedback!

We’re excited to finally get into the open with this project, and look forward to hearing what you
think!

We’re going to use the issue tracking associated with the rubyomr-preview github project to track the
feedback, so if you’d like to tell or ask us anything,
[please open an issue](https://github.com/rubyomr-preview/rubyomr-preview/issues)
