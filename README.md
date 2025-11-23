# CloudHub Documentation

## Table of Contents

- Overview
- Developer Setup
  - Installation
  - Running Locally

## Overview

Documentation for the CloudHub

## Developer Setup

### Installation

1) Update Brew

    ```bash
    brew update brew
    ```

2) Install [MKDocs](https://www.mkdocs.org/) via pip3

    ```bash
    pip3 install mkdocs
    ```

3) Install [Pipenv](https://github.com/pypa/pipenv)

    ```bash
    brew install pipenv
    ```

### Running MKDocs locally

1) Clone the [documentation repo](https://git.rockfin.com/ce-1701/enterprise-documentation)

    ```bash
    git clone https://git.rockfin.com/ce-1701/enterprise-documentation.git
    ```

2) Install/Configure MKDocs

    ```bash
    pipenv install mkdocs
    ```

3) Run MKDocs

    ```bash
    pipenv run mkdocs serve
    ```

4) Browsing MKDocs Site

    ```bash
    http://localhost:8000/
    ```
