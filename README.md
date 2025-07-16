# Static Marks

[![Build](https://img.shields.io/github/actions/workflow/status/darekkay/static-marks/ci.yml?branch=master&style=flat-square)](https://github.com/darekkay/static-marks/actions/workflows/ci.yml)
[![npm](https://img.shields.io/npm/v/static-marks.svg?style=flat-square)](https://www.npmjs.com/package/static-marks)
[![license](https://img.shields.io/github/license/darekkay/static-marks.svg?style=flat-square)](https://github.com/darekkay/static-marks/blob/master/LICENSE)

## Features

- Share your bookmarks via a website. ([demo](https://darekkay.com/static-marks/demo/default.html))



- Search your bookmarks via a `?search=%s` URL param. ([example](https://darekkay.com/static-marks/demo/default.html?search=fire))


  - It can be added into browser as custom search engine.

- Custom web page templates if you don't like the default UI. ([example](https://darekkay.com/static-marks/demo/custom.html) based on [this template](https://github.com/darekkay/static-marks/blob/master/docs/examples/templates/custom.html)).

- Dark Mode.

## Deploy

### Local

- [Install](#installation) Static Marks:

```shell
npm install -g static-marks
```

- Create a [YAML file](#file-format) `bookmarks.yml` containing your bookmarks. Alternatively, convert your browser bookmarks to YAML file:

```shell
# static-marks import browser-bookmarks.html > bookmarks.yml

# 将最新导出的 Chrome 书签转为 yml 文件
ls -U bookmarks_*.html | head -1 | xargs -I {} static-marks import {} > bookmarks.yml
```

- [Build](#build-bookmarks-app) your bookmarks app:

```shell
static-marks build bookmarks.yml > bookmarks.html
```

### Vercel

- Add this to Vercel Project's Build Command, automatically build newest Chrome Bookmarks

```shell
npm install -g static-marks; ls -U bookmarks_*.html | head -1 | xargs -I {} static-marks import {} > bookmarks.yml; static-marks build bookmarks.yml > bookmarks.html
```

- Upload bookmarks to this repository, it will automatically update.

## Usage

```
static-marks [options] <command>

Options:
  -V, --version               output the version number
  -h, --help                  output usage information

Commands:
  build [options] <files...>  build bookmarks app
  import [options] <file>     import bookmarks from chrome, firefox or pocket
  report <files...>           report bookmarks
```

Run `static-marks <command> --help` to view the usage of a specific command.

### Build bookmarks app

```
static-marks build [options] <files...>

Options:
  -o, --output [file]     output to a file (use stdout by default)
  -t, --title [title]     set document title
  --template-file [file]  use a custom web page template
```

Examples:

```bash
static-marks build -o bookmarks.html bookmark.yml  # Single file
static-marks build bookmarks.yml > bookmarks.html  # Alt. notation
static-marks build f1.yml f2.yml > bookmarks.html  # Multiple files
static-marks build files/* > bookmarks.html        # All files at path
```

### Import bookmarks

```
static-marks import [options] <file>

Options:
  -o, --output [file]  output to a file (use stdout by default)
```

Examples:

```bash
static-marks import exported.html > imported.yml
static-marks import -o imported.yml exported.html
```

### View a report for your bookmarks

Currently, the report contains only the total bookmarks count. In the future, it might be used for detecting duplicate and dead links.

```bash
static-marks report [options] <files...>
```

Examples:

```bash
static-marks report bookmarks.yml
static-marks report files/*
```

## File format

Bookmark files are written in [YAML](http://yaml.org). There are multiple levels of hierarchy:

```yaml
Collection:
  - Bucket:
      - Link: https://example.com
```

A link URL can be expressed either as an item property or as a child item:

```yaml
- Link 1: https://example.com
- Link 2:
    - https://example.com
```

Notes and nested links are added as children of a link (the first element is the link URL). If the text is a valid-formatted URL it will be automatically converted to a link:

```yaml
- Link with notes:
    - https://example.com
    - This is a text note
    - Link note: https://example.com
    - https://example.net
```

First-level notes can be used to describe or structure a bucket:

```yaml
- Bucket:
    - Link 1: https://example.com
    - Carpe diem!
```

Here's a complete example:

```yaml
Collection:
  - Bucket:
      - Link 1: https://example.com
      - Link 2:
          - https://example.com
      - Link with notes:
          - https://example.com
          - This is a text note
          - Link note: https://example.com
      - First-level note
```

There is an optional 1st level hierarchy level available when you provide more than one bookmark file to `static-marks`. When passing multiple files, a header menu will be displayed to toggle between individual files and all bookmarks.

```bash
# file toggle menu not necessary/not available
static-marks build file1.yaml

# file toggle menu available
static-marks build file1.yaml file2.yaml
```

## Troubleshooting

### Encoding issues

You can use both `>` to pipe the results of a `static-marks` command or the `-o` to provide an explicit output file. There might be cases where the `>` variant doesn't play well with the shell encoding, though.

See [#46](https://github.com/darekkay/static-marks/issues/46) for more information.

## Using Static Marks with Gitlab Pages

You can leverage [GitLab Pages](https://docs.gitlab.com/ee/user/project/pages/) to host your Static Marks instance. Check out the [example repository](https://gitlab.com/darekkay/static-marks-gitlab-ci) and a [live demo](https://darekkay.gitlab.io/static-marks-gitlab-ci).

1. [Create](https://gitlab.com/projects/new) a new GitLab repository.
2. Include the example [.gitlab-ci.yml](docs/examples/ci/.gitlab-ci.yml) file in the root directory.
3. Add all your bookmark `*.yml` files in a `bookmarks` directory.

After every push to the `master` branch, your Static Marks page will be rebuilt. By default, it will be available at `https://<USERNAME>.gitlab.io/<PROJECTNAME>`.

## Development and Contribution

The frontend part of Static Mark is maintained in [another repository](https://github.com/darekkay/static-marks-app), where a template file (`_template.html`) is being generated. This approach makes it possible to include the whole application and user-defined bookmarks in a single HTML file.

If you want to provide any frontend-related changes, please create a PR in the other repository. Changes to the core CLI application are handled here instead.

## Contributors

Thanks goes to these wonderful people:

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href='https://darekkay.com/' title='darekkay is awesome!'><img src='https://avatars0.githubusercontent.com/u/3101914?v=4' alt='darekkay' width='45px' /></a></td>
    <td align="center"><a href='https://github.com/gaerfield' title='gaerfield is awesome!'><img src='https://avatars0.githubusercontent.com/u/13051868?v=4' alt='gaerfield' width='45px' /></a></td>
    <td align="center"><a href='https://github.com/kaushalyap' title='kaushalyap is awesome!'><img src='https://avatars3.githubusercontent.com/u/24698778?v=4' alt='kaushalyap' width='45px' /></a></td>
    <td align="center"><a href='https://github.com/k-kalinowski' title='k-kalinowski is awesome!'><img src='https://avatars2.githubusercontent.com/u/8605057?v=4' alt='k-kalinowski' width='45px' /></a></td>
    <td align="center"><a href='https://github.com/g33k247' title='g33k247 is awesome!'><img src='https://avatars0.githubusercontent.com/u/8498814?v=4' alt='g33k247' width='45px' /></a></td>
    <td align="center"><a href='https://github.com/dzmitry-lahoda' title='dzmitry-lahoda is awesome!'><img src='https://avatars3.githubusercontent.com/u/757125?v=4' alt='dzmitry-lahoda' width='45px' /></a></td>
    <td align="center"><a href='http://www.diegomunozbeltran.com/' title='diegombeltran is awesome!'><img src='https://avatars2.githubusercontent.com/u/7081281?v=4' alt='diegombeltran' width='45px' /></a></td>
    <td align="center"><a href='http://veereshr.me' title='veerreshr is awesome!'><img src='https://avatars0.githubusercontent.com/u/59141533?v=4' alt='veerreshr' width='45px' /></a></td>
    <td align="center"><a href='https://github.com/nausher' title='nausher is awesome!'><img src='https://avatars3.githubusercontent.com/u/79359?v=4' alt='nausher' width='45px' /></a></td>
    <td align="center"><a href='https://github.com/acer123acer123' title='acer123acer123 is awesome!'><img src='https://avatars3.githubusercontent.com/u/5222071?v=4' alt='acer123acer123' width='45px' /></a></td>
  </tr>
  <tr>
    <td align="center"><a href='https://github.com/jflip' title='jflip is awesome!'><img src='https://avatars1.githubusercontent.com/u/9138082?v=4' alt='jflip' width='45px' /></a></td>
    <td align="center"><a href='https://www.eduardominguez.es' title='e-minguez is awesome!'><img src='https://avatars.githubusercontent.com/u/346758?v=4' alt='e-minguez' width='45px' /></a></td>
    <td align="center"><a href='https://github.com/iaeiou' title='iaeiou is awesome!'><img src='https://avatars.githubusercontent.com/u/69427615?v=4' alt='iaeiou' width='45px' /></a></td>
    <td align="center"><a href='https://majiehong.com' title='Jiehong is awesome!'><img src='https://avatars.githubusercontent.com/u/1061229?v=4' alt='Jiehong' width='45px' /></a></td>
    <td align="center"><a href='https://github.com/RishiKumarRay' title='RishiKumarRay is awesome!'><img src='https://avatars.githubusercontent.com/u/87641376?v=4' alt='RishiKumarRay' width='45px' /></a></td>
    <td align="center"><a href='https://github.com/overflowy' title='overflowy is awesome!'><img src='https://avatars.githubusercontent.com/u/98480250?v=4' alt='overflowy' width='45px' /></a></td>
  </tr>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://allcontributors.org) specification to acknowledge all contributions.

## License

This project and its contents are open source under the [MIT license](https://github.com/darekkay/static-marks/blob/master/LICENSE).
