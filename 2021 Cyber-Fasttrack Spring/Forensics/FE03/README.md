# FE03
## BRIEFING
Download the file and find a way to get the flag from the docker image.

Contents: fe03.tar.gz

## Solution

Unzipping the fe03.tar.gz file we find a whole of bunch of directories with names consisting of numbers and letters.

```console
root@osboxes:~/Downloads/fe03# ls
1f30c30bb8115de38e4435bd872ba4fdd75d4e42c9ef64d73ed2934579e186ab
20f27a62bd4b6360e6d71ee3b6e3d4e23d27f8316853b5f115134dc496b76921
26987f28e329f8358f9c56af97f27b600774ffafc11bcd4cb9cceb9f34f61346
2ed2b63c90d753ae128f90373b24d24a7419e5aa46a89a676480c03b0cefe2bc
3d6ea52838fd9d0b55e12aaf4a35b6f12d64fccd8ba7018d010e388d5b99c643
426c7b404154ef34234fbce5bc72821d25dd35b1b0047543c3d695ac8ce0efa8
51e5b690e5172dfd0b3af22ad2d4efb8d12654c159b57d6de187c383503991b9
6fa81b7764c555365e7bb49698b8a0f44a249fb376239a7201e5dc03fe6a7011
7f7a6df265cf8880f5810dd2f14338a07ff0b1b31d027c0379bd2c4ef89b2d32
a58d6ba385a9ff4e2e1f6778f428d240fac9f08ccf90e869079b185cbf3ab7f6
ad31a16a747a7717698f8924839b8c81e4c084767adbdb7be0e85797ddfc71b0
c46eb315a38f7abc53c600748ca1bb022b571acb74e5d6d6efe16a79914742bf.json
d1f4d5de510b5cfb3aeb03a7a335d8f0b6bde087b445663551a60b2c76371f5d
e03add6569d7d70bdbb076e7bacfef9efd3ad25f59b391a0d0c8df51278e21bc
f02c14991ec079d9803398debd7dded1dae074ff0e3493213b87d9c09e79fde1
fe03.tar.gz
fe03.zip
manifest.json
repositories
```

If we cd into any of these directories we find 3 files: `json`, `layer.tar`, and `VERSION`. I started unzipping the tar files and quickly found the one containing the flag:

```console
root@osboxes:~/Downloads/fe03/20f27a62bd4b6360e6d71ee3b6e3d4e23d27f8316853b5f115134dc496b76921# ls
home  json  layer.tar  VERSION
root@osboxes:~/Downloads/fe03/20f27a62bd4b6360e6d71ee3b6e3d4e23d27f8316853b5f115134dc496b76921# cd home
root@osboxes:~/Downloads/fe03/20f27a62bd4b6360e6d71ee3b6e3d4e23d27f8316853b5f115134dc496b76921/home# ls
secret
root@osboxes:~/Downloads/fe03/20f27a62bd4b6360e6d71ee3b6e3d4e23d27f8316853b5f115134dc496b76921/home# cd secret
root@osboxes:~/Downloads/fe03/20f27a62bd4b6360e6d71ee3b6e3d4e23d27f8316853b5f115134dc496b76921/home/secret# ls
flag.txt
root@osboxes:~/Downloads/fe03/20f27a62bd4b6360e6d71ee3b6e3d4e23d27f8316853b5f115134dc496b76921/home/secret# cat flag.txt 
Flag: 8191-SiMpLeFilESysTemForens1Cs
```

The flag is **8191-SiMpLeFilESysTemForens1Cs**.
