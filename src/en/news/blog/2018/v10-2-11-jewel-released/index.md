---
title: "v10.2.11 Jewel released"
date: "2018-07-11"
author: "TheAnalyst"
tags:
  - "release"
  - "jewel"
---

This point releases brings a number of important bugfixes and has a few important security fixes. This is expected to be the last Jewel release. We recommend all Jewel 10.2.x users to upgrade.

## Notable Changes

- CVE 2018-1128: auth: cephx authorizer subject to replay attack ([issue#24836](http://tracker.ceph.com/issues/24836), Sage Weil)
- CVE 2018-1129: auth: cephx signature check is weak ([issue#24837](http://tracker.ceph.com/issues/24837), Sage Weil)
- CVE 2018-10861: mon: auth checks not correct for pool ops ([issue#24838](http://tracker.ceph.com/issues/24838), Jason Dillaman)
- The RBD C API’s rbd\_discard method and the C++ API’s Image::discard method now enforce a maximum length of 2GB. This restriction prevents overflow of the result code.
- New OSDs will now use rocksdb for omap data by default, rather than leveldb. omap is used by RGW bucket indexes and CephFS directories, and when a single leveldb grows to 10s of GB with a high write or delete workload, it can lead to high latency when leveldb’s single-threaded compaction cannot keep up. rocksdb supports multiple threads for compaction, which avoids this problem.
- The CephFS client now catches failures to clear dentries during startup and refuses to start as consistency and untrimmable cache issues may develop. The new option client\_die\_on\_failed\_dentry\_invalidate (default: true) may be turned off to allow the client to proceed (dangerous!).
- In 10.2.10 and earlier releases, keyring caps were not checked for validity, so the caps string could be anything. As of 10.2.11, caps strings are validated and providing a keyring with an invalid caps string to, e.g., “ceph auth add” will result in an error.

## Changelog

- admin: bump sphinx to 1.6 ([issue#21717](http://tracker.ceph.com/issues/21717), [pr#18166](https://github.com/ceph/ceph/pull/18166), Kefu Chai, Alfredo Deza)
- auth: ceph auth add does not sanity-check caps ([issue#22525](http://tracker.ceph.com/issues/22525), [pr#21367](https://github.com/ceph/ceph/pull/21367), Jing Li, Nathan Cutler, Kefu Chai, Sage Weil)
- build/ops: rpm: bump epoch ahead of ceph-common in RHEL base ([issue#20508](http://tracker.ceph.com/issues/20508), [pr#21190](https://github.com/ceph/ceph/pull/21190), Ken Dreyer)
- build/ops: upstart: radosgw-all does not start on boot if ceph-base is not installed ([issue#18313](http://tracker.ceph.com/issues/18313), [pr#16294](https://github.com/ceph/ceph/pull/16294), Ken Dreyer)
- ceph\_authtool: add mode option ([issue#23513](http://tracker.ceph.com/issues/23513), [pr#21197](https://github.com/ceph/ceph/pull/21197), Sébastien Han)
- ceph-disk: factor out the retry logic into a decorator ([issue#21728](http://tracker.ceph.com/issues/21728), [pr#18169](https://github.com/ceph/ceph/pull/18169), Kefu Chai)
- ceph-disk: fix –runtime omission when enabling [ceph-osd@$ID.service](mailto:ceph-osd%40$ID.service) units for device-backed OSDs ([issue#21498](http://tracker.ceph.com/issues/21498), [pr#17942](https://github.com/ceph/ceph/pull/17942), Carl Xiong)
- ceph-disk flake8 test fails on very old, and very new, versions of flake8 ([issue#22207](http://tracker.ceph.com/issues/22207), [pr#19153](https://github.com/ceph/ceph/pull/19153), Nathan Cutler)
- cephfs: ceph.in: pass RADOS inst to LibCephFS ([issue#21406](http://tracker.ceph.com/issues/21406), [issue#21967](http://tracker.ceph.com/issues/21967), [pr#19907](https://github.com/ceph/ceph/pull/19907), Patrick Donnelly)
- cephfs: client::mkdirs not handle well when two clients send mkdir request for a same dir ([issue#20592](http://tracker.ceph.com/issues/20592), [pr#20271](https://github.com/ceph/ceph/pull/20271), dongdong tao)
- cephfs: client: prevent fallback to remount when dentry\_invalidate\_cb is true but root->dir is NULL ([issue#23211](http://tracker.ceph.com/issues/23211), [pr#21189](https://github.com/ceph/ceph/pull/21189), Zhi Zhang)
- cephfs: fix tmap\_upgrade crash ([issue#23529](http://tracker.ceph.com/issues/23529), [pr#21208](https://github.com/ceph/ceph/pull/21208), “Yan, Zheng”)
- cephfs: fuse client: ::rmdir() uses a deleted memory structure of dentry leads … ([issue#22536](http://tracker.ceph.com/issues/22536), [pr#19993](https://github.com/ceph/ceph/pull/19993), YunfeiGuan)
- cephfs-journal-tool: add “set pool\_id” option ([issue#22631](http://tracker.ceph.com/issues/22631), [pr#20111](https://github.com/ceph/ceph/pull/20111), dongdong tao)
- cephfs-journal-tool: move shutdown to the deconstructor of MDSUtility ([issue#22734](http://tracker.ceph.com/issues/22734), [pr#20333](https://github.com/ceph/ceph/pull/20333), dongdong tao)
- cephfs: osdc: “FAILED assert(bh->last\_write\_tid > tid)” in powercycle-wip-yuri-master-1.19.18-distro-basic-smithi ([issue#22741](http://tracker.ceph.com/issues/22741), [pr#20312](https://github.com/ceph/ceph/pull/20312), “Yan, Zheng”)
- cephfs: osdc/Journaler: make sure flush() writes enough data ([issue#22824](http://tracker.ceph.com/issues/22824), [pr#20435](https://github.com/ceph/ceph/pull/20435), “Yan, Zheng”)
- cephfs: Processes stuck waiting for write with ceph-fuse ([issue#22008](http://tracker.ceph.com/issues/22008), [issue#22207](http://tracker.ceph.com/issues/22207), [pr#19141](https://github.com/ceph/ceph/pull/19141), “Yan, Zheng”)
- ceph-fuse: failure to remount in startup test does not handle client\_die\_on\_failed\_remount properly ([issue#22269](http://tracker.ceph.com/issues/22269), [pr#21162](https://github.com/ceph/ceph/pull/21162), Patrick Donnelly)
- ceph.in: bypass codec when writing raw binary data ([issue#23185](http://tracker.ceph.com/issues/23185), [pr#20763](https://github.com/ceph/ceph/pull/20763), Oleh Prypin)
- ceph-objectstore-tool command to trim the pg log ([issue#23242](http://tracker.ceph.com/issues/23242), [pr#20882](https://github.com/ceph/ceph/pull/20882), Josh Durgin, David Zafman)
- ceph-objectstore-tool: “$OBJ get-omaphdr” and “$OBJ list-omap” scan all pgs instead of using specific pg ([issue#21327](http://tracker.ceph.com/issues/21327), [pr#20284](https://github.com/ceph/ceph/pull/20284), David Zafman)
- ceph.restart + ceph\_manager.wait\_for\_clean is racy ([issue#15778](http://tracker.ceph.com/issues/15778), [pr#20508](https://github.com/ceph/ceph/pull/20508), Warren Usui, Sage Weil)
- ceph\_volume\_client: fix setting caps for IDs ([issue#21501](http://tracker.ceph.com/issues/21501), [pr#18084](https://github.com/ceph/ceph/pull/18084), Ramana Raja)
- class rbd.Image discard—-OSError: \[errno 2147483648\] error discarding region ([issue#16465](http://tracker.ceph.com/issues/16465), [issue#21966](http://tracker.ceph.com/issues/21966), [pr#20287](https://github.com/ceph/ceph/pull/20287), Nathan Cutler, Huan Zhang, Jason Dillaman)
- cli/crushtools/build.t sometimes fails in jenkins’ make check run ([issue#21758](http://tracker.ceph.com/issues/21758), [pr#21158](https://github.com/ceph/ceph/pull/21158), Kefu Chai)
- client reconnect gather race ([issue#22263](http://tracker.ceph.com/issues/22263), [pr#21163](https://github.com/ceph/ceph/pull/21163), “Yan, Zheng”)
- client: release revoking Fc after invalidate cache ([issue#22652](http://tracker.ceph.com/issues/22652), [pr#19975](https://github.com/ceph/ceph/pull/19975), “Yan, Zheng”)
- client: set client\_try\_dentry\_invalidate to false by default ([issue#21423](http://tracker.ceph.com/issues/21423), [pr#17925](https://github.com/ceph/ceph/pull/17925), “Yan, Zheng”)
- \[cli\] rename of non-existent image results in seg fault ([issue#21248](http://tracker.ceph.com/issues/21248), [pr#20280](https://github.com/ceph/ceph/pull/20280), Jason Dillaman)
- CLI unit formatting tests are broken ([issue#24733](http://tracker.ceph.com/issues/24733), [pr#22913](https://github.com/ceph/ceph/pull/22913), Jason Dillaman)
- common: compute SimpleLRU’s size with contents.size() instead of lru.… ([issue#22613](http://tracker.ceph.com/issues/22613), [pr#19978](https://github.com/ceph/ceph/pull/19978), Xuehan Xu)
- common/config: set rocksdb\_cache\_size to OPT\_U64 ([issue#22104](http://tracker.ceph.com/issues/22104), [pr#18850](https://github.com/ceph/ceph/pull/18850), Vikhyat Umrao, liuhongtong)
- common: fix typo in rados bench write JSON output ([issue#24199](http://tracker.ceph.com/issues/24199), [pr#22407](https://github.com/ceph/ceph/pull/22407), Sandor Zeestraten)
- config: lower default omap entries recovered at once ([issue#21897](http://tracker.ceph.com/issues/21897), [pr#19927](https://github.com/ceph/ceph/pull/19927), Josh Durgin)
- core: Addition of online osd ‘omap’compaction command ([issue#19592](http://tracker.ceph.com/issues/19592), [pr#17101](https://github.com/ceph/ceph/pull/17101), liuchang0812, Sage Weil)
- core: global/signal\_handler.cc: fix typo ([issue#21432](http://tracker.ceph.com/issues/21432), [pr#17883](https://github.com/ceph/ceph/pull/17883), Kefu Chai)
- core: librados: Double free in rados\_getxattrs\_next ([issue#22042](http://tracker.ceph.com/issues/22042), [pr#20381](https://github.com/ceph/ceph/pull/20381), Gu Zhongyan)
- core: Objecter::C\_ObjectOperation\_sparse\_read throws/catches exceptions on -ENOENT ([issue#21844](http://tracker.ceph.com/issues/21844), [pr#18743](https://github.com/ceph/ceph/pull/18743), Jason Dillaman)
- Deleting a pool with active notify linger ops can result in seg fault ([issue#23966](http://tracker.ceph.com/issues/23966), [pr#22188](https://github.com/ceph/ceph/pull/22188), Kefu Chai, Jason Dillaman)
- doc: clarify Path Restriction instructions ([issue#16906](http://tracker.ceph.com/issues/16906), [pr#19795](https://github.com/ceph/ceph/pull/19795), huanwen ren)
- doc: clarify Path Restriction instructions ([issue#16906](http://tracker.ceph.com/issues/16906), [pr#19840](https://github.com/ceph/ceph/pull/19840), Drunkard Zhang)
- doc: remove region from INSTALL CEPH OBJECT GATEWAY ([issue#21610](http://tracker.ceph.com/issues/21610), [pr#18303](https://github.com/ceph/ceph/pull/18303), Orit Wasserman)
- Filestore rocksdb compaction readahead option not set by default ([issue#21505](http://tracker.ceph.com/issues/21505), [pr#20446](https://github.com/ceph/ceph/pull/20446), Mark Nelson)
- follow-on: osd: be\_select\_auth\_object() sanity check oi soid ([issue#20471](http://tracker.ceph.com/issues/20471), [pr#20622](https://github.com/ceph/ceph/pull/20622), David Zafman)
- HashIndex: randomize split threshold by a configurable amount ([issue#15835](http://tracker.ceph.com/issues/15835), [pr#19906](https://github.com/ceph/ceph/pull/19906), Josh Durgin)
- include/fs\_types: fix unsigned integer overflow ([issue#22494](http://tracker.ceph.com/issues/22494), [pr#19611](https://github.com/ceph/ceph/pull/19611), runsisi)
- install-deps.sh: point gcc to the one shipped by distro ([issue#22220](http://tracker.ceph.com/issues/22220), [pr#19461](https://github.com/ceph/ceph/pull/19461), Kefu Chai)
- install-deps.sh: readlink /usr/bin/gcc not /usr/bin/x86\_64-linux-gnu-gcc ([issue#22220](http://tracker.ceph.com/issues/22220), [pr#19521](https://github.com/ceph/ceph/pull/19521), Kefu Chai)
- install-deps.sh: update g++ symlink also ([issue#22220](http://tracker.ceph.com/issues/22220), [pr#19656](https://github.com/ceph/ceph/pull/19656), Kefu Chai)
- journal: Message too long error when appending journal ([issue#23526](http://tracker.ceph.com/issues/23526), [pr#21215](https://github.com/ceph/ceph/pull/21215), Mykola Golub)
- \[journal\] tags are not being expired if no other clients are registered ([issue#21960](http://tracker.ceph.com/issues/21960), [pr#20282](https://github.com/ceph/ceph/pull/20282), Jason Dillaman)
- legal: remove doc license ambiguity ([issue#23336](http://tracker.ceph.com/issues/23336), [pr#20999](https://github.com/ceph/ceph/pull/20999), Nathan Cutler)
- librados: copy out data to users’ buffer for xio ([issue#20616](http://tracker.ceph.com/issues/20616), [pr#17594](https://github.com/ceph/ceph/pull/17594), Vu Pham)
- librbd: cannot clone all image-metas if we have more than 64 key/value pairs ([issue#21814](http://tracker.ceph.com/issues/21814), [pr#21228](https://github.com/ceph/ceph/pull/21228), PCzhangPC)
- librbd: cannot copy all image-metas if we have more than 64 key/value pairs ([issue#21815](http://tracker.ceph.com/issues/21815), [pr#21203](https://github.com/ceph/ceph/pull/21203), PCzhangPC)
- librbd: create+truncate for whole-object layered discards ([issue#23285](http://tracker.ceph.com/issues/23285), [pr#21219](https://github.com/ceph/ceph/pull/21219), Jason Dillaman)
- librbd: list\_children should not attempt to refresh image ([issue#21670](http://tracker.ceph.com/issues/21670), [pr#21224](https://github.com/ceph/ceph/pull/21224), Jason Dillaman)
- librbd: object map batch update might cause OSD suicide timeout ([issue#22716](http://tracker.ceph.com/issues/22716), [issue#21797](http://tracker.ceph.com/issues/21797), [pr#21220](https://github.com/ceph/ceph/pull/21220), Song Shun, Jason Dillaman)
- librbd: set deleted parent pointer to null ([issue#22158](http://tracker.ceph.com/issues/22158), [pr#19098](https://github.com/ceph/ceph/pull/19098), Jason Dillaman)
- log: Fix AddressSanitizer: new-delete-type-mismatch ([issue#23324](http://tracker.ceph.com/issues/23324), [pr#21084](https://github.com/ceph/ceph/pull/21084), Brad Hubbard)
- mds: FAILED assert(get\_version() < pv) in CDir::mark\_dirty ([issue#21584](http://tracker.ceph.com/issues/21584), [pr#21156](https://github.com/ceph/ceph/pull/21156), Yan, Zheng, “Yan, Zheng”)
- mds: fix dump last\_sent ([issue#22562](http://tracker.ceph.com/issues/22562), [pr#19961](https://github.com/ceph/ceph/pull/19961), dongdong tao)
- mds: fix integer overflow ([issue#21067](http://tracker.ceph.com/issues/21067), [pr#17188](https://github.com/ceph/ceph/pull/17188), Henry Chang)
- mds: fix scrub crash ([issue#22730](http://tracker.ceph.com/issues/22730), [pr#20335](https://github.com/ceph/ceph/pull/20335), dongdong tao)
- mds: session reference leak ([issue#22821](http://tracker.ceph.com/issues/22821), [pr#21175](https://github.com/ceph/ceph/pull/21175), Nathan Cutler, “Yan, Zheng”)
- mds: unbalanced auth\_pin/auth\_unpin in RecoveryQueue code ([issue#22647](http://tracker.ceph.com/issues/22647), [pr#20067](https://github.com/ceph/ceph/pull/20067), “Yan, Zheng”)
- mds: underwater dentry check in CDir::\_omap\_fetched is racy ([issue#23032](http://tracker.ceph.com/issues/23032), [pr#21185](https://github.com/ceph/ceph/pull/21185), Yan, Zheng)
- mon/LogMonitor: call no\_reply() on ignored log message ([issue#24180](http://tracker.ceph.com/issues/24180), [pr#22431](https://github.com/ceph/ceph/pull/22431), Sage Weil)
- mon/MDSMonitor: no\_reply on MMDSLoadTargets ([issue#23769](http://tracker.ceph.com/issues/23769), [pr#22189](https://github.com/ceph/ceph/pull/22189), Sage Weil)
- mon/OSDMonitor.cc: fix expected\_num\_objects interpret error ([issue#22530](http://tracker.ceph.com/issues/22530), [pr#22050](https://github.com/ceph/ceph/pull/22050), Yang Honggang)
- mon/OSDMonitor: fix dividing by zero in OSDUtilizationDumper ([issue#22662](http://tracker.ceph.com/issues/22662), [pr#20344](https://github.com/ceph/ceph/pull/20344), Mingxin Liu)
- ObjectStore/StoreTest.FiemapHoles/3 fails with kstore ([issue#21716](http://tracker.ceph.com/issues/21716), [pr#20143](https://github.com/ceph/ceph/pull/20143), Kefu Chai, Ning Yao)
- osd: also check the exsistence of clone obc for “CEPH\_SNAPDIR” requests ([issue#17445](http://tracker.ceph.com/issues/17445), [pr#17707](https://github.com/ceph/ceph/pull/17707), Xuehan Xu)
- osdc/Objecter: prevent double-invocation of linger op callback ([issue#23872](http://tracker.ceph.com/issues/23872), [pr#21754](https://github.com/ceph/ceph/pull/21754), Jason Dillaman)
- osd: objecter sends out of sync with pg epochs for proxied ops ([issue#22123](http://tracker.ceph.com/issues/22123), [pr#20518](https://github.com/ceph/ceph/pull/20518), Sage Weil)
- osd ops (sent and?) arrive at osd out of order ([issue#19133](http://tracker.ceph.com/issues/19133), [issue#19139](http://tracker.ceph.com/issues/19139), [pr#17893](https://github.com/ceph/ceph/pull/17893), Jianpeng Ma, Sage Weil)
- osd: OSDMap cache assert on shutdown ([issue#21737](http://tracker.ceph.com/issues/21737), [pr#21184](https://github.com/ceph/ceph/pull/21184), Greg Farnum)
- osd: osd\_scrub\_during\_recovery only considers primary, not replicas ([issue#18206](http://tracker.ceph.com/issues/18206), [pr#17815](https://github.com/ceph/ceph/pull/17815), David Zafman)
- osd/PrimaryLogPG: dump snap\_trimq size ([issue#22448](http://tracker.ceph.com/issues/22448), [pr#21200](https://github.com/ceph/ceph/pull/21200), Piotr Dałek)
- osd: recover\_replicas: object added to missing set for backfill, but is not in recovering, error! ([issue#18162](http://tracker.ceph.com/issues/18162), [issue#14513](http://tracker.ceph.com/issues/14513), [pr#18690](https://github.com/ceph/ceph/pull/18690), huangjun, Adam C. Emerson, David Zafman)
- osd: replica read can trigger cache promotion ([issue#20919](http://tracker.ceph.com/issues/20919), [pr#21199](https://github.com/ceph/ceph/pull/21199), Sage Weil)
- osd: update heartbeat peers when a new OSD is added ([issue#18004](http://tracker.ceph.com/issues/18004), [pr#20108](https://github.com/ceph/ceph/pull/20108), Pan Liu)
- performance: Only scan for omap corruption once ([issue#21328](http://tracker.ceph.com/issues/21328), [pr#18951](https://github.com/ceph/ceph/pull/18951), David Zafman)
- qa: failures from pjd fstest ([issue#21383](http://tracker.ceph.com/issues/21383), [pr#21152](https://github.com/ceph/ceph/pull/21152), “Yan, Zheng”)
- qa: src/test/libcephfs/test.cc:376: Expected: (len) > (0), actual: -34 vs 0 ([issue#22221](http://tracker.ceph.com/issues/22221), [pr#21172](https://github.com/ceph/ceph/pull/21172), Patrick Donnelly)
- qa: use xfs instead of btrfs w/ filestore ([issue#20169](http://tracker.ceph.com/issues/20169), [issue#20911](http://tracker.ceph.com/issues/20911), [pr#18165](https://github.com/ceph/ceph/pull/18165), Sage Weil)
- qa: use xfs instead of btrfs w/ filestore ([issue#21481](http://tracker.ceph.com/issues/21481), [pr#17847](https://github.com/ceph/ceph/pull/17847), Patrick Donnelly)
- radosgw: fix awsv4 header line sort order ([issue#21607](http://tracker.ceph.com/issues/21607), [pr#18080](https://github.com/ceph/ceph/pull/18080), Marcus Watts)
- rbd: clean up warnings when mirror commands used on non-setup pool ([issue#21319](http://tracker.ceph.com/issues/21319), [pr#21227](https://github.com/ceph/ceph/pull/21227), Jason Dillaman)
- rbd: disk usage on empty pool no longer returns an error message ([issue#22200](http://tracker.ceph.com/issues/22200), [pr#19186](https://github.com/ceph/ceph/pull/19186), Jason Dillaman)
- \[rbd\] image-meta list does not return all entries ([issue#21179](http://tracker.ceph.com/issues/21179), [pr#20281](https://github.com/ceph/ceph/pull/20281), Jason Dillaman)
- rbd: is\_qemu\_running in qemu\_rebuild\_object\_map.sh and qemu\_dynamic\_features.sh may return false positive ([issue#23502](http://tracker.ceph.com/issues/23502), [pr#21207](https://github.com/ceph/ceph/pull/21207), Mykola Golub)
- rbd: \[journal\] allocating a new tag after acquiring the lock should use on-disk committed position ([issue#22945](http://tracker.ceph.com/issues/22945), [pr#21206](https://github.com/ceph/ceph/pull/21206), Jason Dillaman)
- rbd: librbd: filter out potential race with image rename ([issue#18435](http://tracker.ceph.com/issues/18435), [pr#19855](https://github.com/ceph/ceph/pull/19855), Jason Dillaman)
- rbd ls -l crashes with SIGABRT ([issue#21558](http://tracker.ceph.com/issues/21558), [pr#19801](https://github.com/ceph/ceph/pull/19801), Jason Dillaman)
- rbd-mirror: cluster watcher should ensure it has latest OSD map ([issue#22461](http://tracker.ceph.com/issues/22461), [pr#19644](https://github.com/ceph/ceph/pull/19644), Jason Dillaman)
- rbd-mirror: fix potential infinite loop when formatting status message ([issue#22932](http://tracker.ceph.com/issues/22932), [pr#20418](https://github.com/ceph/ceph/pull/20418), Mykola Golub)
- rbd-mirror: ignore permission errors on rbd\_mirroring object ([issue#20571](http://tracker.ceph.com/issues/20571), [pr#21225](https://github.com/ceph/ceph/pull/21225), Jason Dillaman)
- rbd-mirror: strip environment/CLI overrides for remote cluster ([issue#21894](http://tracker.ceph.com/issues/21894), [pr#21223](https://github.com/ceph/ceph/pull/21223), Jason Dillaman)
- \[rbd-nbd\] Fedora does not register resize events ([issue#22131](http://tracker.ceph.com/issues/22131), [pr#19115](https://github.com/ceph/ceph/pull/19115), Jason Dillaman)
- rbd-nbd: fix ebusy when do map ([issue#23528](http://tracker.ceph.com/issues/23528), [pr#21232](https://github.com/ceph/ceph/pull/21232), Li Wang)
- rbd: possible deadlock in various maintenance operations ([issue#22120](http://tracker.ceph.com/issues/22120), [pr#20285](https://github.com/ceph/ceph/pull/20285), Jason Dillaman)
- rbd: rbd crashes during map ([issue#21808](http://tracker.ceph.com/issues/21808), [pr#18843](https://github.com/ceph/ceph/pull/18843), Peter Keresztes Schmidt)
- rbd: rbd-mirror split brain test case can have a false-positive failure until teuthology ([issue#22485](http://tracker.ceph.com/issues/22485), [pr#21205](https://github.com/ceph/ceph/pull/21205), Jason Dillaman)
- rbd: TestLibRBD.RenameViaLockOwner may still fail with -ENOENT ([issue#23068](http://tracker.ceph.com/issues/23068), [pr#20627](https://github.com/ceph/ceph/pull/20627), Mykola Golub)
- repair\_test fails due to race with osd start ([issue#20705](http://tracker.ceph.com/issues/20705), [pr#20146](https://github.com/ceph/ceph/pull/20146), Sage Weil)
- rgw: 15912 15673 (Fix duplicate tag removal during GC, cls/refcount: store and use list of retired tags) ([issue#20107](http://tracker.ceph.com/issues/20107), [pr#16708](https://github.com/ceph/ceph/pull/16708), Jens Rosenboom)
- rgw: abort in listing mapped nbd devices when running in a container ([issue#22012](http://tracker.ceph.com/issues/22012), [issue#22011](http://tracker.ceph.com/issues/22011), [pr#20286](https://github.com/ceph/ceph/pull/20286), Li Wang, Pan Liu)
- rgw: add ability to sync user stats from admin api ([issue#21301](http://tracker.ceph.com/issues/21301), [pr#20179](https://github.com/ceph/ceph/pull/20179), Nathan Johnson)
- rgw: add cors header rule check in cors option request ([issue#22002](http://tracker.ceph.com/issues/22002), [pr#19057](https://github.com/ceph/ceph/pull/19057), yuliyang)
- rgw: add radosgw-admin sync error trim to trim sync error log ([issue#23287](http://tracker.ceph.com/issues/23287), [pr#21210](https://github.com/ceph/ceph/pull/21210), fang yuxiang)
- rgw: add xml output header in RGWCopyObj\_ObjStore\_S3 response msg ([issue#22416](http://tracker.ceph.com/issues/22416), [pr#19887](https://github.com/ceph/ceph/pull/19887), Enming Zhang)
- rgw: automated trimming of datalog and mdlog ([issue#18227](http://tracker.ceph.com/issues/18227), [pr#20061](https://github.com/ceph/ceph/pull/20061), Casey Bodley)
- rgw: bi list entry count incremented on error, distorting error code ([issue#21205](http://tracker.ceph.com/issues/21205), [pr#18207](https://github.com/ceph/ceph/pull/18207), Nathan Cutler)
- rgw: boto3 v4 SignatureDoesNotMatch failure due to sorting of sse-kms headers ([issue#21832](http://tracker.ceph.com/issues/21832), [pr#18772](https://github.com/ceph/ceph/pull/18772), Nathan Cutler)
- rgw: bucket resharding should not update bucket ACL or user stats ([issue#22124](http://tracker.ceph.com/issues/22124), [pr#20421](https://github.com/ceph/ceph/pull/20421), Orit Wasserman)
- rgw: copying part without http header x-amz-copy-source-range will be mistaken for copying object ([issue#22729](http://tracker.ceph.com/issues/22729), [pr#21294](https://github.com/ceph/ceph/pull/21294), Malcolm Lee)
- rgw: core dump, recursive lock of RGWKeystoneTokenCache ([issue#23171](http://tracker.ceph.com/issues/23171), [pr#20639](https://github.com/ceph/ceph/pull/20639), Mark Kogan, Adam Kupczyk)
- rgw: data sync of versioned objects, note updating bi marker ([issue#18885](http://tracker.ceph.com/issues/18885), [pr#21213](https://github.com/ceph/ceph/pull/21213), Yehuda Sadeh)
- rgw: dont log EBUSY errors in ‘sync error list’ ([issue#22473](http://tracker.ceph.com/issues/22473), [pr#19908](https://github.com/ceph/ceph/pull/19908), Casey Bodley)
- rgw: ECANCELED in rgw\_get\_system\_obj() leads to infinite loop ([issue#17996](http://tracker.ceph.com/issues/17996), [pr#20561](https://github.com/ceph/ceph/pull/20561), Yehuda Sadeh)
- rgw: file deadlock on lru evicting ([issue#22736](http://tracker.ceph.com/issues/22736), [pr#20076](https://github.com/ceph/ceph/pull/20076), Matt Benjamin)
- rgw: file write error ([issue#21455](http://tracker.ceph.com/issues/21455), [pr#18304](https://github.com/ceph/ceph/pull/18304), Yao Zongyou)
- rgw: fix chained cache invalidation to prevent cache size growth ([issue#22410](http://tracker.ceph.com/issues/22410), [pr#19469](https://github.com/ceph/ceph/pull/19469), Mark Kogan)
- rgw: fix doubled underscore with s3/swift server-side copy ([issue#22529](http://tracker.ceph.com/issues/22529), [pr#19747](https://github.com/ceph/ceph/pull/19747), Matt Benjamin)
- rgw: fix GET website response error code ([issue#22272](http://tracker.ceph.com/issues/22272), [pr#19488](https://github.com/ceph/ceph/pull/19488), Dmitry Plyakin)
- rgw: fix index update in dir\_suggest\_changes ([issue#24280](http://tracker.ceph.com/issues/24280), [pr#22677](https://github.com/ceph/ceph/pull/22677), Tianshan Qu)
- rgw: fix marker encoding problem ([issue#20463](http://tracker.ceph.com/issues/20463), [pr#17731](https://github.com/ceph/ceph/pull/17731), Orit Wasserman, Marcus Watts)
- rgw: fix swift anonymous access ([issue#22259](http://tracker.ceph.com/issues/22259), [pr#19194](https://github.com/ceph/ceph/pull/19194), Marcus Watts)
- rgw: Fix swift object expiry not deleting objects ([issue#22084](http://tracker.ceph.com/issues/22084), [pr#18925](https://github.com/ceph/ceph/pull/18925), Pavan Rallabhandi)
- rgw: fix the bug that part’s index can’t be removed after completing ([issue#19604](http://tracker.ceph.com/issues/19604), [pr#16763](https://github.com/ceph/ceph/pull/16763), Zhang Shaowen, Matt Benjamin)
- rgw: fix the max-uploads parameter not work ([issue#22825](http://tracker.ceph.com/issues/22825), [pr#20479](https://github.com/ceph/ceph/pull/20479), Xin Liao)
- rgw: inefficient buffer usage for PUTs ([issue#23207](http://tracker.ceph.com/issues/23207), [pr#21098](https://github.com/ceph/ceph/pull/21098), Marcus Watts)
- rgw: libcurl & ssl fixes ([issue#22951](http://tracker.ceph.com/issues/22951), [issue#23203](http://tracker.ceph.com/issues/23203), [issue#23162](http://tracker.ceph.com/issues/23162), [pr#20749](https://github.com/ceph/ceph/pull/20749), Marcus Watts, Abhishek Lekshmanan, Jesse Williamson)
- rgw: list bucket which enable versioning get wrong result when user marker ([issue#21500](http://tracker.ceph.com/issues/21500), [pr#20291](https://github.com/ceph/ceph/pull/20291), yuliyang)
- rgw: log includes zero byte sometimes ([issue#20037](http://tracker.ceph.com/issues/20037), [pr#17151](https://github.com/ceph/ceph/pull/17151), Abhishek Lekshmanan)
- rgw: make init env methods return an error ([issue#23039](http://tracker.ceph.com/issues/23039), [pr#20800](https://github.com/ceph/ceph/pull/20800), Abhishek Lekshmanan)
- RGW: Multipart upload may double the quota ([issue#21586](http://tracker.ceph.com/issues/21586), [pr#18121](https://github.com/ceph/ceph/pull/18121), Sibei Gao, Matt Benjamin)
- rgw: multisite: data sync status advances despite failure in RGWListBucketIndexesCR ([issue#21735](http://tracker.ceph.com/issues/21735), [pr#20269](https://github.com/ceph/ceph/pull/20269), Casey Bodley)
- rgw: multisite: Get bucket location which is located in another zonegroup, will return 301 Moved Permanently ([issue#21125](http://tracker.ceph.com/issues/21125), [pr#18305](https://github.com/ceph/ceph/pull/18305), Shasha Lu, lvshuhua, Jiaying Ren)
- rgw: null instance mtime incorrect when enable versioning ([issue#21743](http://tracker.ceph.com/issues/21743), [pr#20262](https://github.com/ceph/ceph/pull/20262), Shasha Lu)
- rgw: radosgw-admin: add an option to reset user stats ([issue#23335](http://tracker.ceph.com/issues/23335), [issue#23322](http://tracker.ceph.com/issues/23322), [pr#20877](https://github.com/ceph/ceph/pull/20877), Abhishek Lekshmanan)
- rgw: release cls lock if taken in RGWCompleteMultipart ([issue#21596](http://tracker.ceph.com/issues/21596), [issue#22368](http://tracker.ceph.com/issues/22368), [pr#18116](https://github.com/ceph/ceph/pull/18116), Casey Bodley, Matt Benjamin)
- rgw: resharding needs to set back the bucket ACL after link ([issue#22742](http://tracker.ceph.com/issues/22742), [pr#20039](https://github.com/ceph/ceph/pull/20039), Orit Wasserman)
- rgw: resolve Random 500 errors in Swift PutObject (22517) ([issue#22517](http://tracker.ceph.com/issues/22517), [issue#21560](http://tracker.ceph.com/issues/21560), [pr#19769](https://github.com/ceph/ceph/pull/19769), Adam C. Emerson, Matt Benjamin)
- rgw: rgw\_file: recursive lane lock can occur in LRU drain ([issue#20374](http://tracker.ceph.com/issues/20374), [pr#17149](https://github.com/ceph/ceph/pull/17149), Matt Benjamin)
- rgw: S3 POST policy should not require Content-Type ([issue#20201](http://tracker.ceph.com/issues/20201), [pr#19635](https://github.com/ceph/ceph/pull/19635), Matt Benjamin)
- rgw: s3website error handler uses original object name ([issue#23201](http://tracker.ceph.com/issues/23201), [issue#20307](http://tracker.ceph.com/issues/20307), [pr#21100](https://github.com/ceph/ceph/pull/21100), liuhong, Casey Bodley)
- rgw: segfaults after running radosgw-admin data sync init ([issue#22083](http://tracker.ceph.com/issues/22083), [pr#19783](https://github.com/ceph/ceph/pull/19783), Casey Bodley, Abhishek Lekshmanan)
- rgw: segmentation fault when starting radosgw after reverting .rgw.root ([issue#21996](http://tracker.ceph.com/issues/21996), [pr#20292](https://github.com/ceph/ceph/pull/20292), Orit Wasserman, Casey Bodley)
- rgw: stale bucket index entry remains after object deletion ([issue#22555](http://tracker.ceph.com/issues/22555), [pr#20293](https://github.com/ceph/ceph/pull/20293), J. Eric Ivancich)
- rgw: system user can’t delete bucket completely ([issue#22248](http://tracker.ceph.com/issues/22248), [pr#21212](https://github.com/ceph/ceph/pull/21212), Casey Bodley)
- rgw: tcmalloc ([issue#23469](http://tracker.ceph.com/issues/23469), [pr#21073](https://github.com/ceph/ceph/pull/21073), Matt Benjamin)
- rgw: upldate the max-buckets when the quota is uploaded ([issue#22745](http://tracker.ceph.com/issues/22745), [pr#20496](https://github.com/ceph/ceph/pull/20496), zhaokun)
- rgw: user creation can overwrite existing user even if different uid is given ([issue#21685](http://tracker.ceph.com/issues/21685), [pr#20074](https://github.com/ceph/ceph/pull/20074), Casey Bodley)
- RHEL 7.3 Selinux denials at OSD start ([issue#19200](http://tracker.ceph.com/issues/19200), [pr#18780](https://github.com/ceph/ceph/pull/18780), Boris Ranto)
- scrub errors not cleared on replicas can cause inconsistent pg state when replica takes over primary ([issue#23267](http://tracker.ceph.com/issues/23267), [pr#21194](https://github.com/ceph/ceph/pull/21194), David Zafman)
- snapset xattr corruption propagated from primary to other shards ([issue#20186](http://tracker.ceph.com/issues/20186), [issue#18409](http://tracker.ceph.com/issues/18409), [issue#21907](http://tracker.ceph.com/issues/21907), [pr#20331](https://github.com/ceph/ceph/pull/20331), David Zafman)
- systemd: Add explicit Before=ceph.target ([issue#21477](http://tracker.ceph.com/issues/21477), [pr#17841](https://github.com/ceph/ceph/pull/17841), Tim Serong)
- table of contents doesn’t render for luminous/jewel docs ([issue#23780](http://tracker.ceph.com/issues/23780), [pr#21503](https://github.com/ceph/ceph/pull/21503), Alfredo Deza)
- test: Adjust for Jewel quirk caused of differences with master ([issue#23006](http://tracker.ceph.com/issues/23006), [pr#20463](https://github.com/ceph/ceph/pull/20463), David Zafman)
- test/CMakeLists: disable test\_pidfile.sh ([issue#20975](http://tracker.ceph.com/issues/20975), [pr#20557](https://github.com/ceph/ceph/pull/20557), Sage Weil)
- test\_health\_warnings.sh can fail ([issue#21121](http://tracker.ceph.com/issues/21121), [pr#20289](https://github.com/ceph/ceph/pull/20289), Sage Weil)
- test/librbd: fixed metadata tests under upgrade scenarios ([issue#21911](http://tracker.ceph.com/issues/21911), [pr#18548](https://github.com/ceph/ceph/pull/18548), Jason Dillaman)
- test/librbd: utilize unique pool for cache tier testing ([issue#11502](http://tracker.ceph.com/issues/11502), [pr#20524](https://github.com/ceph/ceph/pull/20524), Jason Dillaman)
- tests: rbd\_mirror\_helpers.sh request\_resync\_image function saves image id to wrong variable ([issue#21663](http://tracker.ceph.com/issues/21663), [pr#19804](https://github.com/ceph/ceph/pull/19804), Jason Dillaman)
- tests: test\_admin\_socket.sh may fail on wait\_for\_clean ([issue#23499](http://tracker.ceph.com/issues/23499), [pr#21125](https://github.com/ceph/ceph/pull/21125), Mykola Golub)
- tests: tests/librbd: updated test\_notify to handle new release lock semantics ([issue#21912](http://tracker.ceph.com/issues/21912), [pr#18560](https://github.com/ceph/ceph/pull/18560), Jason Dillaman)
- tests: unittest\_pglog timeout ([issue#23504](http://tracker.ceph.com/issues/23504), [issue#18030](http://tracker.ceph.com/issues/18030), [pr#21135](https://github.com/ceph/ceph/pull/21135), Nathan Cutler, Loic Dachary)
- tools: ceph-objectstore-tool set-size should clear data-digest ([issue#22112](http://tracker.ceph.com/issues/22112), [pr#20070](https://github.com/ceph/ceph/pull/20070), David Zafman)
- Ubuntu amd64 client can not discover the ubuntu arm64 ceph cluster ([issue#19705](http://tracker.ceph.com/issues/19705), [pr#18294](https://github.com/ceph/ceph/pull/18294), Kefu Chai)
