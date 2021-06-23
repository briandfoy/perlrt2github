# perlrt2github

Perl 5 Porters used the RT issue tracker for a couple decades, then
switched to GitHub in October 2019. They imported all of the old
issues to GitHub and provided a mapping from old to new issue numbers.
The old RT instance redirects to GitHub. The original data are at 
[https://rt-archive.perl.org/perl5/rtgithub.csv](https://rt-archive.perl.org/perl5/rtgithub.csv)
and in many cases it might be easier to simply look up the value.

I had a very simple way to deal with this, but I then had an idea 
for employing [String::Sprintf](https://metacpan.org/pod/String::Sprintf) 
to allow people to make the output anything they wanted. This program 
is probably more interesting as an example for doing this sort
of thing.

Aside from this program, which is more fun than useful, I use my
[3xx](https://github.com/briandfoy/3xx) tool to see the web redirects 
that end up at GitHub:

    $ 3xx https://rt.perl.org/perl5/Ticket/Display.html?id=827
    https://github.com/Perl/perl5/issues/26
    
Some examples from the docs:

	% perlrt2github -1 827
	26

	% perlrt2github -2 827
	827	26

	% perlrt2github -c 827
	827,26

	% perl perlrt2github -f 'GitHub => %G' 827
	GitHub => https://github.com/Perl/perl5/issues/26

	% perlrt2github -j 827
	{"rt": { "id": 827, "url": "https://rt.perl.org/perl5/Ticket/Display.html?id=827" }, "github": { "id": 26, "url": "https://github.com/Perl/perl5/issues/26" } }

	% perlrt2github -j 827 | jq -r
	{
	  "rt": {
		"id": 827,
		"url": "https://rt.perl.org/perl5/Ticket/Display.html?id=827"
	  },
	  "github": {
		"id": 26,
		"url": "https://github.com/Perl/perl5/issues/26"
	  }
	}

	% perlrt2github -j 827 | jq -r .github
	{
	  "id": 26,
	  "url": "https://github.com/Perl/perl5/issues/26"
	}

	% perlrt2github -j 827 | jq -r .github.url
	https://github.com/Perl/perl5/issues/26

	% perlrt2github 827
	RT:     https://rt.perl.org/perl5/Ticket/Display.html?id=827
	Github: https://github.com/Perl/perl5/issues/26

	% perlrt2github -t 827
	827	26

	% perlrt2github -x 827
	<?xml version="1.0" encoding="UTF-8" ?>
	<map>
		<rt id="827" url="https://rt.perl.org/perl5/Ticket/Display.html?id=827" />
		<github id="26" url="https://github.com/Perl/perl5/issues/26" />
	</map>

	% perlrt2github -x 827 | xmllint --xpath 'string(//rt/@url)' -
	https://rt.perl.org/perl5/Ticket/Display.html?id=827

	% perlrt2github -x 827 | xpath -q -e 'string(//rt/@url)'
	https://rt.perl.org/perl5/Ticket/Display.html?id=827
