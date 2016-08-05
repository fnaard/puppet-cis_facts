# Overview

This repository contains a prototype or proof-of-concept.  Its aim is to provide Facter facts that reflect a node's compliance with CIS benchmarks.  Currently this is only implemented for nodes running

  * Redhat-family Enterprise Linux 6

# Philosophy

Perfect is the enemy of a functional bunch of pokes at the problem.  Rather than aim for a "product," let's just throw this out and see what happens.

Make it easy for the less-technical folks to read.  All facts in this project use the actual commands that the CIS guidelines list for each item.  We could implement the facts using "real" classes like File.  To an auditor, however, using the actual commands that they're looking for is way easier than trying to explain to them what cleverness you're doing in Ruby code.

Don't dither.  When you mention this idea to someone, they invariably start brainstorming.  Theoretical discussion is fun!  But this does not result in code or pull requests.

Make it super easy to discover and report nodes' status.  Using facts means that a person can use an `mco` command with a cleverly-crafted `--with-fact` flag to quickly get a report of machines that are failing some certain test.  Or you could do a PuppetDB query.  Or look in the Enterprise Console.  Using facts makes these things easy.


# Implementation

## Ruby-based facts

Most individual guidelines just use Facter::Core::Execution.exec() to run the command listed in the benchmark document.  Based on the result of that command, if the guideline passes nothing is done, and if the guideline is failed, its identity is added to an array of failing guidelines.

For the RedHat benchmark, the arrays of failing guidelines are broken up into multiple facts that span an X.Y section of the guideline.  For other systems, like AIX, the arrays should span an X.Y.Z section of their respective guideline.  The goal is to have a dozen or so checks' status collapsed into each sub-section grouping.

## Facts.d-based facts

Some audit commands are just too heavy to execute on every run (think of ones that do a find down the whole filesystem) and those are implemented by cron jobs that write their results to facts.d.

# Getting started

To distribute these facts to nodes, simply install the module on your Puppet master.  On your next run, all of the facts will get synced down to all agents and they'll start to show up, even if you do not classify nodes with a cis class.  You only need to include classes to get the cron facts.d-based facts.

## Mcollective Ad-Hoc Queries

Let's add some example mco queries that will let someone easily get basic info on their environment.

## PuppetDB Ad-Hoc Queries

Let's add some example curls to get folks started.
