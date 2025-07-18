<script lang="ts">
  import {
    Composer,
    ContentEditable,
    ActionBar,
    RichTextPlugin,
    HistoryPlugin,
    ListPlugin,
    CheckListPlugin,
    HorizontalRulePlugin,
    ImagePlugin,
    TEXT_FORMAT_TRANSFORMERS,
    ELEMENT_TRANSFORMERS,
    HR,
    IMAGE,
    CHECK_LIST,
  } from '$lib/index.js';
  import {
    HeadingNode,
    QuoteNode,
    ListNode,
    ListItemNode,
    HorizontalRuleNode,
    ImageNode,
  } from '$lib/index.js';
  import {theme as editorTheme} from '$lib/themes/system-light-dark/index.js';
  import {
    $getRoot as getRoot,
    $createTextNode as createTextNode,
    $createParagraphNode as createParagraphNode,
  } from '$lib/index.js';
  import MarkdownShortcutPlugin from '$lib/core/plugins/MarkdownShortcut/MarkdownShortcutPlugin.svelte';
  import Composer3Toolbar from './Composer3Toolbar.svelte';

  const initialConfig = {
    theme: editorTheme,
    namespace: 'pg_sveltekit',
    nodes: [
      HeadingNode,
      ListNode,
      ListItemNode,
      QuoteNode,
      HorizontalRuleNode,
      ImageNode,
    ],
    onError: (error: Error) => {
      throw error;
    },
    editorState: () => {
      const root = getRoot();
      if (root.getFirstChild() === null) {
        const paragraph = createParagraphNode();
        paragraph.append(
          createTextNode('This demo environment is built with '),
          createTextNode('svelte-lexical').toggleFormat('code'),
          createTextNode('.'),
          createTextNode(' Try typing in '),
          createTextNode('some text').toggleFormat('bold'),
          createTextNode(' with '),
          createTextNode('different').toggleFormat('italic'),
          createTextNode(' formats.'),
        );
        root.append(paragraph);
      }
    },
  };
</script>

<Composer {initialConfig}>
  <div class="editor-shell svelte-lexical">
    <Composer3Toolbar />
    <div class="editor-container">
      <div class="editor-scroller">
        <div class="editor">
          <ContentEditable />
        </div>
      </div>
      <RichTextPlugin />
      <HistoryPlugin />
      <ListPlugin />
      <CheckListPlugin />
      <HorizontalRulePlugin />
      <ImagePlugin captionsEnabled={false} />
      <MarkdownShortcutPlugin
        transformers={[
          ...TEXT_FORMAT_TRANSFORMERS,
          ...ELEMENT_TRANSFORMERS,
          HR,
          IMAGE,
          CHECK_LIST,
        ]} />

      <ActionBar />
    </div>
  </div>
</Composer>
