# PH-Composer library provider example

A separate SketchUp extension that registers two synthetic Materials through PH-Composer's
public provider contract. The values illustrate the contract and are not product specifications.
The provider needs no account, network connection or PH-Composer source changes.

Requires a PH-Composer development build with the Library and Assemblies features and contract
version 1. This example uses the Library API under development in
[PH-Composer](https://github.com/ph-composer/ph-composer-skp). The Ruby source is compatible with Ruby 2.7.
This repository does not establish a public
SketchUp support or extension-release claim.

## Install

Copy `bldgtyp_example_library.rb` and the sibling `bldgtyp_example_library/` directory into your
SketchUp Plugins directory. Enable PH-Composer, then enable **BLDGTYP Example Library** in
Extension Manager. Its two records appear under the **BLDGTYP synthetic example** provider.

If the example loads before PH-Composer finishes, it prints a diagnostic and registers nothing.
After PH-Composer finishes, enable the example or call this in the Ruby Console:

```ruby
BldgtypExampleLibrary.activate
```

Duplicate registration is refused. The provider serves Materials only.

## Panel-closed walkthrough

Open a synthetic Model, close the PH-Composer panel, and load this repository's `walkthrough.rb`
in SketchUp's Ruby Console using its actual absolute path:

```ruby
load "/absolute/path/to/ph-composer-library-provider-example/walkthrough.rb"
proof = BldgtypExampleLibraryWalkthrough.run
raise "Example failed" unless proof.fetch("passed")
```

The walkthrough connects to the active Model with explicit grants, loads the provider through
the setup API and lists its records,
previews and applies one Material copy using the revision and proposal digest, then reads it
back. The first copy is one Model operation. An unchanged prior copy is reused without a write;
a conflicting local record requires review before the script proceeds. The copied record remains
editable in the Model. Use SketchUp Undo to reverse a new copy.

## Files and validation

`bldgtyp_example_library.rb` is the extension loader. `bldgtyp_example_library/install.rb`
declares the feature and provider; `walkthrough.rb` uses the public API. The provider's list is
hard-coded synthetic data in SI and returns immutable batches. It never receives a Model.

PH-Composer's `tests/example_library_provider_test.rb` exercises early-load diagnostics, the
shared provider contract, duplicate/incompatible registration, unsupported kinds, and copy/reuse
read-back. For a syntax check, run `ruby -c` on each of the three Ruby files with Ruby 2.7.

## License and contributions

Copyright 2026 BLDGTYP, LLC and contributors. Code and documentation are licensed under
[AGPL-3.0-or-later](LICENSE). Contributions use the [Developer Certificate of Origin](DCO.md);
sign off every commit with `git commit -s`. Do not contribute licensed or private project data.
