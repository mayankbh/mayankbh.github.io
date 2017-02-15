---
layout: post
title: Setting up and running PowerGraph on Ubuntu
category: technology
date: 2017-02-15
---

## PowerGraph setup

Do note that the same instructions also apply for PowerLyra (possibly +- a broken link).

PowerGraph is a graph processing framework that leverages vertex cuts (divide vertices among multiple nodes) to improve performance on power law graphs.

Setting it up can be a bit of a pain, however. These instructions are so that I don't forget, and hopefully others can avoid the same mistakes I made. I performed this setup on Ubuntu 16.04, but I believe it works on 10.04, 12.04 and 14.04 as well.

1. Clone the git repository at https://github.com/jegonzal/PowerGraph

1. Install the dependencies that it recommends for Ubuntu (for other distros (Arch ;\_;), I hope there are equivalent packages, or build from source I suppose)

1. Run ./configure

1. The CMakeLists.txt file provides the list of dependencies and can install them locally for you. Unfortunately, some of the links 404 when you try installing them. As I recall, they are-
    - libevent
    - gperftools

1. You'll have to update the URL/URL\_MD5 bits in the CMakeLists.txt file to reflect the new URLs/checksums (I really don't know why I had to change the checksum for the same version, but such is the way of life)

1. Powergraph provides a bunch of toolkits you can build, you don't really need to build all of them at a time (unless you have plenty of that). For my use case, I only needed the Graph Analytics framework, so navigate into {release,debug}/toolkits/graph\_analytics (You can choose either the release or the debug build, of course). You can now run make -j<num> to run make with $num concurrent jobs. This'll take a while, so go get some coffee/dinner.

1. Pray that compiled, and come back to a bunch of executables in the folder, hopefully. 

1. Repeat the above process for each node, or use the mpirsync command suggested to copy the installation. If you're using emulab, you can create a custom image from this node and then use it to spawn more nodes with PowerGraph already installed, yay.

1. Edit your ~/machines file with the IPs/hostnames of various machines in your cluster (each one should be able to SSH into the other without a password)

1. Run the sample command that PowerGraph suggests, for instance, pagerank on a synthetic dataset.

1. It's probably worth trying to generate the documentation as well. Just navigate to doc/ and run doxygen, which should generate a huge bunch of html files that you can happily bookmark and navigate.

## Generating datasets

There's a neat tool to generate synthetic power law graphs at https://www-complexnetworks.lip6.fr/~latapy/FV/generation.html

1. Just download and build it, then you can run bin/distrib to generate the distribution, and bin/graph to generate the actual graph from the given distribution. Funny thing, it sometimes segfaults on me on the second last vertex of the graph, on Ubuntu, but works fine on OS X. where it's apparently not been tested (Why, Linus, whyyy?).

1. This generates a graph file with an adjacency list where each line looks like-
\<vertex\> \<neighbour 1\>...\<neighbour n\>

1. Unfortunately, PowerGraph has a fixed set of input formats it can take in, and this isn't one of them. It needs to be modified for each line to look like-

\<vertex\>\<num\_neghbours\> \<neighbour 1\>...\<neighbour n\>

1. Which should be manageable with a simple $scripting\_language\_of\_your\_choice script.

1. To mix things up, you can randomize the vertices (initially they're sorted by number of neighbours) using sort -R

1. You can also split the file into multiple parts so that the ingestion can be divided (I haven't tried this yet, though)

1. Finally, PowerGraph can take input from HDFS or local files. If they're local files, ensure they're in the same path in all the nodes, otherwise things get ugly. I tried this in Emulab, so I'd recommend installing it on one node, generating the dataset, then creating a custom image from the node so that you can easily setup a cluster with all nodes ready to SSH into each other and PowerGraph already installed.

## Care with emulab directories

If you try to install in your home directory on Emulab, it tries to build/install the PowerGraph files to your home directory in the ZFS share, which 1) has fixed quotas, and 2) feels slower to me. Instead, you can use the mkextrafs command that EmuLab provides to use the 4th disk partition (which gives you around 100GB to play with, which is not persistent across nodes, but works fine for experimentation purposes)
