# What is this?

When authenticating with a password, this passes along literal backspaces. In
the status quo, if you type something in like `supersecuer<BS><BS>repassword`
it is interpreted as `supersecurepassword` and the backspaces correct your typos.

With these pam modifications, your password is the literal
`supersecuer<BS><BS>repassword`, and you must make the typo and the two
backspaces in order for your password to be accepted.

# How do I use this?

This is a modified version of `libpam_misc`. After building, use
`LD_PRELOAD=build/libpam_misc/libpam_misc.so.0 passwd` to change your password
to something with spaces. Then you can run /bin/login with the same `LD_PRELOAD`
or you can also replace your system `libpam_misc.so.0` if you want for added
security out-of-the-box.

# Do I actually want this?

No.


# Upstream install notes

How to use it is as follows:

Please look at the ci/install-dependencies.sh for the necessary
prerequisite packages to be able to build the Linux-PAM. The script
is targeted at Debian based Linux distributions so the package
names and availability might differ on other distributions.

First, configure the build using meson setup:

      mkdir build
      meson setup <your-options> build

Then compile:

      meson compile -C build

To make sure everything was compiled correct, run:

      meson test -C build

If a test fails, you should not continue to install this build.
These tests require a suitable file /etc/pam.d/other; if necessary,
create such a file containing, e.g., these five lines (not indented)

	#%PAM-1.0
	auth	 required	pam_deny.so
	account	 required	pam_deny.so
	password required	pam_deny.so
	session	 required	pam_deny.so


Note, if you are worried - don't even think about doing the next line
(most Linux distributions already support PAM out of the box, so if
something goes wrong with installing the code from this version your
box may stop working..)

      meson install -C build

That said, please report problems to the bug reporting database
at https://github.com/linux-pam/linux-pam/issues .

To generate manual pages from the XML source files you need the
docbook-xsl stylesheets in version 1.69.1 or newer, older versions had
a bug which generates a broken layout.
