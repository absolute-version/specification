absolute-version specification
-------------------------------

This document is the specification for the absolute-version format. 

For the rationale and motivation of this versioning strategy, see the readme of [absolute-version](https://github.com/absolute-version/absolute-version-js).

The absolute-version specification is a semantic extension of [semver](https://semver.org/) at version 2.0.0. All absolute-version versions are semver compliant.

## Specification

* Version of this specification: 1.0.0-alpha


### Backus–Naur Form Grammar

Extending the definitions [here](https://semver.org/spec/v2.0.0.html#backusnaur-form-grammar-for-valid-semver-versions), with the prefix `semver` on terms not defined in this document:

```
<valid abs-ver> ::= <released version>
                  | <abs-ver pre-release version>

<released version> ::= <semver version core> 
                     | <semver version core> "-" <semver pre-release>
                  
<abs-ver pre-release> ::= <semver version core> "-" <branch name> "+" <git and build info>
                       | <semver version core> "-" <semver pre-release> "." <branch name> "+" <git and build info>

<branch name> ::= <semver pre-release identifier>

<git and buildinfo> ::= <commits since release> "." <git sha>
                      | <commits since release> "." <git sha> "." <semver build identifiers>
                      | <dirty git and buildinfo>

<dirty git and buildinfo> ::= <commits since release> "." <git sha> "." "DIRTY" "." <hostname>
                            | <commits since release> "." <git sha> "." "DIRTY" "." <hostname> "." <semver build identifiers>

<commits since release> ::= <semver numeric identifier>

<git sha> ::= <hex digit> <hex digit> <hex digit> <hex digit> <hex digit> <hex digit> <hex digits>

<hex digits> ::= <hex digit>
               | <hex digit> <hex digits>

<hex digit> ::= "a" | "b" | "c" | "d" | "e" | "f" | "0" | "1" 
              | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"

<hostname> ::= <semver alphanumeric identifier>
```

> `semver numeric identifier` and `semver alphanumeric identifer` are what you expect, but not defined here in the interest of berevity.

Note that the above is not a departure from the grammar of semver, it simply
defines where specific pre-release or build information strings go.

### Specification

As in the semver specification, the key words “MUST”, “MUST NOT”, “REQUIRED”,
“SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and
“OPTIONAL” in this document are to be interpreted as described in [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119).


1. Software using absolute-version MUST meet the criteria in Semantic Version 2.0.0 and MUST also be using
   git or another source control system where branches and commits have similar meanings to git.

1. A version for software that is released MUST follow the `released version` pattern above and MUST NOT
   contain any build information. It MAY contain pre-release identifiers. It is RECOMMENDED that only one
   pre-release identifier is used.

2. A version for software that is between releases MUST follow the `abs-ver pre-release` pattern above.
   It MUST contain the branch identifier, even on the main branch. It MUST contain the number of commits
   since release, and it MUST contain the short git sha generated at the time of release.

3. IF the git working tree was dirty at the time the version was generated, the build metadata MUST contain the
   string `.DIRTY.` and the hostname where the version was generated. It is RECOMMENDED that any other 
   information that might help identify the source of the build (such as username) is included in the semver build metadata.

3. Since the characters allowed in semver pre-release identifiers (`[^a-zA-Z0-9-]`) are more restrictive 
   than git branch names, characters outside the set allowed in semver MUST be removed. It is RECOMMENDED 
   that these are replaced with "-".

3. Since the characters allowed in semver build identifiers (`[^a-zA-Z0-9-]`) are more restrictive than hostnames,
    characters outside the set allowed in semver MUST be removed. It is RECOMMENDED that these are replaced with "-".


### About

This specification was originally written by [Timothy Jones](https://github.com/TimothyJones), to produce unique versions from 
each git commit that are easier for humans to use than git shas, but without losing any machine-readable properties.

If you have questions or suggestions, please [open an issue](https://github.com/absolute-version/specification/issues/new)

### Compliant implementations

If you would like to use absolute-version numbers in your project, the
[absolute-version](https://github.com/absolute-version/absolute-version-js/)
tool can be installed via npm or run without installing via npx.
