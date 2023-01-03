# RPM

- [List RPM content](#list-rpm-content)
- [Extract RPMs](#extract-rpms)
- [Create patches](#create-patches)
- [RPM directory structure](#rpm-directory-structure)
- [Show Config](#show-config)
- [Other RPM commands](#other-rpm-commands)
- [RPM BUILDING](#rpm-building)
- [References](#references)
  - [RPM queries](#rpm-queries)
  - [Spec file](#spec-file)
  - [Building Software with FPM](#building-software-with-fpm)
  - [Fedora Documentation](#fedora-documentation)
  - [rpmforge Repo](#rpmforge-repo)
  - [Create a RPM repo server](#create-a-rpm-repo-server)

## List RPM content

```
rpm -qlp package.rpm
```

## Extract RPMs

```
rpm2cpio package.src.rpm | cpio --extract --make-directories --preserve-modification-time --verbose
```

## Create patches

```
diff -uNr ProgramDir ProgramDir_new > my-changes.patch
mv my-changes.patch ~/rpm-system/SOURCES/.
```

## RPM directory structure

SOURCES — Contains the original sources, patches, and icon files.
SPECS — Contains the spec files used to control the build process.
BUILD — The directory in which the sources are unpacked, and the software is built.
RPMS — Contains the binary package files created by the build process.
SRPMS — Contains the source package files created by the build process.

topdir is the base of several directories, including $RPM_SOURCE_DIR, and $RPM_BUILD_DIR. It defaults to /usr/src/redhat, but it is common to redefine it in ~/.rpmmacros.

builddir ($RPM_BUILD_DIR) is where the source code is unpacked (/usr/src/redhat/BUILD by default), and buildroot ($RPM_BUILD_ROOT) is where the source is installed (typically /var/tmp/%{name}-%{version}-%{release} or something like that). You unpack, ./configure, and make in builddir and then make install to buildroot. RPM collects the files in %files from buildroot.

## Show Config

```
rpm --showrc
```

## Other RPM commands

```
rpm -qa
rpm -qf /usr/bin/mysqlaccess
rpm -qdf /usr/bin/mysqlaccess
rpm -qi MySQL-client
rpm -qip MySQL-client-3.23.57-1.i386.rpm
rpm -qlp ovpc-2.1.10.rpm
rpm -qRp MySQL-client-3.23.57-1.i386.rpm
rpm -qsp MySQL-client-3.23.57-1.i386.rpm
```

## RPM BUILDING

**Folders**

```
/etc/rpmdevtools
/usr/lib/rpm
/usr/share/doc/rpm-[VERSION]
```

List of macros: `/usr/lib/rpm/macros`
Print the macros' value: `rpm --eval %{binder}`

```
rpm --eval %{buildroot}
```

```
/home/rpmbuilder/rpmbuild/BUILDROOT/%{name}-%{version}-%{release}.x86_64
```

The list of all the spec templates: `rpmdev-newspec --help`

The list of all the groups: `less /usr/share/doc/rpm-*/GROUPS`

## References

### RPM queries

- http://ftp.rpm.org/max-rpm/s1-rpm-query-parts.html

### Spec file

- http://www.rpm.org/max-rpm/s1-rpm-build-creating-spec-file.html

### Building Software with FPM

- https://www.digitalocean.com/community/tutorials/how-to-use-fpm-to-easily-create-packages-in-multiple-formats

### Fedora Documentation

= https://docs.fedoraproject.org/ro/Fedora_Draft_Documentation/0.1/html/RPM_Guide/ch09s04.html

### rpmforge Repo

- http://wiki.centos.org/AdditionalResources/Repositories/RPMForge

### Create a RPM repo server

- http://www.serverlab.ca/tutorials/linux/network-services/creating-a-yum-repository-server-for-red-hat-and-centos/
