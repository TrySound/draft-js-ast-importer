# Draft.js AST Importer

Allows you to import the abstract syntax tree (AST) output from the companion [`draft-js-ast-exporter`](https://github.com/icelab/draft-js-ast-exporter). Together they form the full cycle of exporting content from a Draft.js editor instance and then re-importing it.

## Why?

Draft.js supports exporting its content JSON, but this format is a little awkward. Blocks of text are disconnected from their style and entity ranges, and the depth of blocks isn’t implicit. So when it comes to rendering that content in contexts outside a Draft.js editor, you need to have an understanding of how those ranges should be applied and how blocks fit together.

The AST generated by `draft-js-ast-exporter` mitigates this issue by joining common ranges together into marked `inline` or `entity` sections, and by allowing blocks to be nested within one another based on their depth.

## Installation

```
npm install --save draft-js-ast-importer
```

## Usage

```js
import {Editor} from 'draft-js'
import importer from 'draft-js-ast-importer'
import yourDecorator from './decorator'

class Foo extends React.Component {
  constructor(props) {
    super(props)
    const contentState = importer(ast)
    this.state {
      editorState: EditorState.createWithContent(contentState, yourDecorator)
    }
  },

  onChange (editorState) {
    this.setState({editorState})
  },

  render () {
    const {editorState} = this.state
    return (
      <Editor editorState={editorState} onChange={this.onChange}/>
    )
  }
}
```