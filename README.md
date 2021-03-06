# Oy [![npm version](https://badge.fury.io/js/oy-vey.svg)](http://badge.fury.io/js/oy-vey) [![Build Status](https://travis-ci.org/revivek/oy.svg?branch=master)](https://travis-ci.org/revivek/oy)

*Emails, oy vey!*

React components and utilities for server-side HTML email template construction. Oy provides utilities to:

- Move backwards in time to render HTML4, since React only supports HTML5.
- Validate attributes based on best-practices.
- Render your templates server-side.

[Blog Post](http://oyster.engineering/post/124868558323/emails-oy-vey-render-emails-with-react)

## Installation

```
npm install --save oy-vey
```

## Architecture

Oy provides the tools necessary to fill the gaps that React can’t 
(and probably shouldn’t) to help us render email templates on the server.

![Oy Architecture](https://s3.amazonaws.com/oyster-web-static/oy-architecture.jpg)

## Example usage

### 1. Custom Oy components

```js
// MyTable

import React from 'react';
import Oy from 'oy-vey';


export default React.createClass({
  displayName: 'MyTable',

  propTypes: {
    bgColor: Oy.PropTypes.rules(['SixCharacterHexBackgroundColorRule']),
    border: Oy.PropTypes.rules(['TableBorderRule']),
    cellPadding: Oy.PropTypes.rules(['TableCellPaddingRule']),
    cellSpacing: Oy.PropTypes.rules(['TableCellSpacingRule'])
  },

  render: function() {
    return <Oy.Element type="table" {...this.props} />;
  }
});
```


### 2. Modules

```js
import React from 'react';

import MyTable from './my/MyTable.jsx';
import MyTD from './my/MyTD.jsx';
import MyTR from './my/MyTR.jsx';


export default React.createClass({
  displayName: 'BodyText',

  propTypes: {
    maxWidth: React.PropTypes.string.isRequired
  },

  render: function() {
    return (
      <MyTable width={this.props.maxWidth}>
        <MyTR>
          <MyTD align="center">
            {this.props.children}
          </MyTD>
        </MyTR>
      </MyTable>
    );
  }
});
```

### 3. Template

```js
import React from 'react';

import OyLayout from './layout/OyLayout.jsx';

import BodyText from './modules/BodyText.jsx';


export default React.createClass({
  displayName: 'GettingStartedEmail',

  render: function() {
    return (
      <OyLayout>
        <BodyText>Welcome to Oy!</BodyText>
      </OyLayout>
    );
  }
});
```


### 4. Server

Using Express.js:

```js
import express from 'express';
import React from 'react';
import Oy from 'oy-vey';

import GettingStartedEmail from './templates/GettingStartedEmail.jsx';


var server = express();
server.set('port', (process.env.PORT || 8887));

server.get('/email/oy', (req, res) => {
  const template = Oy.renderTemplate({
    title: 'Getting Started with Foo',
    headCSS: '@media ...',
    bodyContent: React.renderToStaticMarkup(<GettingStartedEmail />)
  });
  res.send(template);
});

server.listen(server.get('port'), () => {
  console.log('Node server is running on port', server.get('port'));
});
```


## Contributing

```
# Test
npm test

# Compile from ES6 in src/ to ES5 in lib/
npm run compile
```

We welcome contributions. If there's some information missing or ideas for how to make Oy better, please
send a pull request, file an issue, or email [patelvivek@google.com](mailto:patelvivek@google.com).

The best place to start would be in contributing new rules. [A running wishlist of email validation rules are in the Issues section](https://github.com/oysterbooks/oy/issues?q=is%3Aopen+is%3Aissue+label%3A%22rule+wishlist%22).
