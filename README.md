# containers/shortnames

# Short-Name Aliasing

## What is a short name

When tools like [Podman][podman-gh] or [Docker][docker-cli-gh] pull container images, users prefer to use
short names like `fedora` or `alpine` rather then fully specified image names
`registry.fedoraproject.org/fedora` and `docker.io/alpine`, respectively. In
container engines that allow you to specify more then a single registry for
storing container images, using short names can lead to ambiguity.

Imagine that I have two registries defined and both contain an image named
"foobar". Now if I specify `foobar`, I am not sure which image I will
get. There is potential for malicious parties to take advantage of this by
spoofing images and tricking users into pulling them.

We are building into these container engine tools the ability to use short name
aliases to help mitigate the risk of pulling the wrong image, especially when
the image is a well known short name.

Similar to aliases in shells such as Bash, a short-name alias has a left-hand name that is
being replaced with a right-hand value. Short-name aliases can now be
configured in the 'registries.conf' file as follows:

```
unqualified-search-registries=["registry.fedoraproject.org", "docker.io"]

[aliases]
"fedora"="registry.fedoraproject.org/fedora"
```

All aliases must be specified in the new “aliases” table. Using the above
'registries.conf' file, Podman will resolve “fedora” immediately and securely to
“registry.fedoraproject.org/fedora”. We are currently assembling a public list
of short-name aliases that can be used across the community. Multiple Linux
distributions and companies have expressed interest in collaborating with the
container engines, to help registry their images.

## Goal

The goal of this REPO is to gather a list of shortnames from the community, to
allow distributions to ship them in their distributions. The idea is this list
could be added to the default 'registries.conf' file shipped by a distro.

This list is in the open to guarantee fairness.  We do not want this to be a
free for all land grab, so we will base the list of images on well images
at well known registries and distributions.

In the case of a conflicts, we will base the shortname on the original source of
the image.  For example if the Fedora image is available at 'docker.io' as well
as at 'registry.fedoraproject.org', we will grab it from fedoraproject.

I am sure over time their might be further rules designed if this turns out to
be a problem.

Of course distributions are always free to make changes to this list if/when
they ship it.

## Contributing

Please verify that you are not conflicting with existing shortnames, or state
your case on why your shortname should replace the existing short names.

## Shipping and Packaging Short-Name Aliases

The configuration file is intended to be shipped in the `/etc/containers/registries.conf.d` directory.  This directory allows for supporting drop-in `regististries.conf` configuration files that are loaded in alpha-numerical order.  Users can easily add new files manually or via config managers such as Ansible or Salt.  Please refer to the [upstream documentation of the containers/image library](https://github.com/containers/image/tree/master/docs) to read more about the `registries.conf` format, and the loading behavior.

## Contact

- IRC: #[containers](ircs://irc.libera.chat:6697/#containers) on libera.chat ([Webchat](https://web.libera.chat/))

[podman-gh]:      https://github.com/containers/podman  "GitHub: containers/podman"
[docker-cli-gh]:  https://github.com/docker/cli         "GitHub: docker/cli"
