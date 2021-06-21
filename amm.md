## Launch a particular version of ammonite
The issue with ammonite is that it awalys launches against the latest version of Scala. This makes it extremely difficult to use ammonite with scripts build with older version of Scala. There however are following two approaches to address this problem:
1. Install the particular version of ammonite that could run the script in concern
2. Use [coursier](https://get-coursier.io/) to run specific Ammonite / scala combinations:

    ```sh
    $ cs launch ammonite:2.0.4 --scala 2.12.8
    or
    $ cs launch ammonite --scala 2.13
    ```
    Both full and binary version are accepted as scala versions, and the Ammonite version is optional. Refer this [github issue](https://github.com/com-lihaoyi/Ammonite/issues/1093).
