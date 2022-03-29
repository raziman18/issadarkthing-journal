---
title: "Introduction to ipfs"
date: 2022-03-29T12:49:17+08:00
tags: ["technology"]
---

Imagine an unlimited cloud storage. Where you can store as much data as
possible without having to pay a single cent? This is very useful to those who
wants to share a piece of file that may exceed the upload limit of any cloud
storage services out there.

{{< figure 
src="https://res.cloudinary.com/hiremap/image/upload/v1648535381/ipfs_gfqhis.webp"
>}}

You can take a look at a new technology called InterPlanetary File System
(IPFS). IPFS is a peer-to-peer distributed file system. Much like torrent but
better since it also incentivize nodes who owns the data to ensure data 
persistence, the nodes will be incentivized by a cryptocurrency namely Filecoin.
ipfs is developed in go programming language and the project is open source. 

Let's make a quick demo to show ipfs in action:

Adding a file to the ipfs is as simple as running a single command line 
`ipfs add <file>`
{{< figure
src="https://res.cloudinary.com/hiremap/image/upload/v1648532104/sample_pfdwxd.png" 
width="80%"
caption="uploading aizawa image to ipfs"
>}}

To download the file from ipfs, we need to know the **cid** of the file. The
output of running `ipfs add` command returns the **cid**. We can use that to
retrieve the whole file from the ipfs back to our local filesystem using 
`ipfs cat <cid> > sample.png`.

Using `sxiv` an image viewer tool can display our uploaded file on ipfs.

{{< figure 
src="https://res.cloudinary.com/hiremap/image/upload/v1648531701/screenshot-29-03-2022_13_17_54_osxusn.png"
width="80%"
>}}

Link to the project repository [https://github.com/ipfs](https://github.com/ipfs)

Here are the resources you can learn more about ipfs:
  - [https://ipfs.io/](https://ipfs.io/)
  - [https://docs.ipfs.io/](https://docs.ipfs.io/)


