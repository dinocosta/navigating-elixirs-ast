---
theme: neversink
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Navigating Elixir's AST
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
transition: fade-out
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# open graph
seoMeta:
  # By default, Slidev will use ./og-image.png if it exists,
  # or generate one from the first slide if not found.
  ogImage: auto
  # ogImage: https://cover.sli.dev
fonts:
  sans: 'Inter, Space Mono'
  mono: Space Mono
layout: intro
color: violet
align: lm-lm
---

# Navigating Elixir's AST

<h3 style="font-family: 'Inter'; font-weight: 300;">Credo and Sourceror's Building Blocks</h3>

<div class="absolute bottom-8 left-0 w-full flex flex-col items-left">
    <a href="https://github.com/dinocosta" target="_blank" class="pl-6">
        <carbon-logo-github class="text-xl"/> dinocosta
    </a>
    <a href="https://twitter.com/dinocosta_" target="_blank" class="pl-6 hover:color-green">
        <carbon-logo-twitter class="text-xl"/> dinocosta_
    </a>
</div>

<style>
a {
    @apply color-violet-300;
    border-bottom: none;
}

a:hover {
    border-bottom: none;
    @apply color-violet-100;

}
</style>

---
layout: side-title
color: violet
align: cm-lm
---

::title::

# What is an AST?

::content::

<v-clicks>

- Tree-Like Data Structure
- Represents the code's syntatic structure
- Each node can represent an operation, expression or statement

</v-clicks>

<!--
- The representation abstracts away specific syntax details, like punctutation or formatting
- Provides a structure that makes it easier to analyze and manipulate code programmatically
- Helpful in compilting, interpreting and static code analysis
-->

---
layout: two-cols-title
layoutClass: gap-4
---

::title::

<h1 class="color-violet">What is an AST?</h1>

::left::

```elixir {all|1,5|1,5|2,4|2,4|2,4|3|3|all}{at:1}
defmodule Example do
  def add(x, y) do
    x + y
  end
end
```

::right::

````md magic-move {lines:false}
```elixir
@type ast :: node
@type node :: {atom, [node]}
```
```elixir
@type ast :: node
@type node :: {atom, [node]}

{:defmodule, []}
```
```elixir
@type ast :: node
@type node :: {atom, [node]}

{:defmodule, [
  {:Example, []}
]}
```
```elixir
@type ast :: node
@type node :: {atom, [node]}

{:defmodule, [
  {:Example, []},
  {:def, []}
]}
```
```elixir
@type ast :: node
@type node :: {atom, [node]}

{:defmodule, [
  {:Example, []},
  {:def, [
    {:add, []}
  ]}
]}
```
```elixir
@type ast :: node
@type node :: {atom, [node]}

{:defmodule, [
  {:Example, []},
  {:def, [
    {:add, [
      {:x, []}
      {:y, []}
    ]}
  ]}
]}
```
```elixir
@type ast :: node
@type node :: {atom, [node]}

{:defmodule, [
  {:Example, []},
  {:def, [
    {:add, [
      {:x, []}
      {:y, []}
    ]},
    {:+, []}
  ]}
]}
```
```elixir
@type ast :: node
@type node :: {atom, [node]}

{:defmodule, [
  {:Example, []},
  {:def, [
    {:add, [
      {:x, []}
      {:y, []}
    ]},
    {:+, [
      {:x, []},
      {:y, []}
    ]}
  ]}
]}
```
````

<!--
- Take this piece of Elixir code as an example
- Provides a structure that makes it easier to analyze and manipulate code programmatically
- Helpful in compilting, interpreting and static code analysis
-->

---
layout: side-title
color: violet
align: cm-lm
---

::title::

# Elixir's AST

::content::

<h2 v-click><code>Code</code></h2>

<ul v-click="2">
    <li><code>Code.string_to_quoted/1</code></li>
</ul>

<v-click at="2">
```elixir
Code.string_to_quoted("1 + 1")

{:ok, {:+, [line: 1], [1, 1]}}
```
</v-click>

<ul v-click="3">
    <li><code>Code.eval_string/1</code></li>
</ul>

<v-click at="3">
```elixir
Code.eval_string("1 + 1")

{2, []}
```
</v-click>

<ul v-click="4">
    <li><code>Code.eval_quoted/1</code></li>
</ul>

<v-click at="4">
```elixir
"1 + 1"
|> Code.string_to_quoted!()
|> Code.eval_quoted()

{2, []}
```
</v-click>

<style>
h2 > code {
    @apply color-violet;
    @apply bg-violet-100;
}
</style>

<!--
- Elixir's standard library provides multiple functions in both the `Macro` and
`Code` modules that make it easier to work with Elixir's AST.

- `Code.string_to_quoted/1` – Convert string to AST
- `Code.eval_string/1` – Evaluate string of Elixir code
- `Code.eval_quoted/1` – Evaluate AST
-->

---
layout: side-title
color: violet
align: cm-lm
---

::title::

# Elixir's AST

::content::

<h2 v-click><code>Macro</code></h2>

<ul v-click="2">
    <li><code>Macro.to_string/1</code></li>
</ul>

<v-click at="2">
```elixir
Macro.to_string({:+, [line: 1], [1, 1]}})

"1 + 1"
```
</v-click>

<ul v-click="3">
    <li><code>Macro.prewalk/2</code></li>
</ul>

<v-click at="3">
```elixir
Macro.prewalk({:+, [line: 1], [1, 1]}}, &IO.inspect/1)

# Outputs
{:+, [line: 1], [1, 1]}
1
1

# Returns
{:+, [line: 1], [1, 1]}
```
</v-click>

<style>
h2 > code {
    @apply color-violet;
    @apply bg-violet-100;
}
</style>

<!--
- Elixir's standard library provides multiple functions in both the `Macro` and
`Code` modules that make it easier to work with Elixir's AST.

- `Macro.to_string/1` – Convert AST to String
- `Macro.prewalk/2` – Traverse AST and apply function to each node
-->

---
layout: two-cols-title
layoutClass: gap-4
---

::title::

<div class="flex w-full items-center">
    <h1 class="color-violet grow">Elixir's AST</h1>
    <p class="text-right"><code>Code.string_to_quoted/1</code></p>
</div>

::left::

```elixir {all|2,6|3,5|4}{at:1}
Code.string_to_quoted!("""
defmodule Example do
  def add(x, y) do
    x + y
  end
end
""")
```

::right::

```elixir {all|1-4,20-22|5-12,18-19|13-16}{at:1}
{:defmodule, [line: 1],
  [
    {:__aliases__, [line: 1], [:Example]},
    [ do:
      {:def, [line: 2], [
        {
          :add, [line: 2], [
            {:x, [line: 2], nil},
            {:y, [line: 2], nil}
          ]
        },
        [ do:
            {:+, [line: 3], [
              {:x, [line: 3], nil},
              {:y, [line: 3], nil}
            ]
          }
        ]
      ]}
    ]
  ]
}
```

<style>
div > p > code {
    @apply color-violet;
    @apply bg-violet-100;
}
</style>

<!--
- Circling back to our example from before
- You can see how Elixir's actual AST structure looks a little bit different
- Each tuple has three elements, with the middle one being metadata for the node
- When there's no more children, `nil` is used instead, and `do` blocks are
defined as keyword lists
-->

---
layout: side-title
color: violet
align: cm-lm
---

::title::

# AST Traversal

::content::

<v-clicks>

- Inspect Nodes
- Collect Information
- Apply Transformations

</v-clicks>

---
layout: side-title
color: violet
align: cm-lm
---

::title::

<div class="flex flex-col gap-4">
    <h1>AST Traversal</h1>
    <h3 style="font-family: 'Inter'; font-weight: 300">Unsafe String To Atom</h3>
</div>

::content::

<v-clicks>

- Leverage `Macro.prewalk/2` to traverse the AST
- Find calls to `String.to_atom/1`
- Prints message when calls are found

</v-clicks>

<!--
- The Erlang Virtual Machine has a maximum number of atoms of 1_048_576 .
- Calling `String.to_atom/1` can lead to memory exhaustion, killing the VM.
-->

---
layout: default
---

<div class="flex w-full items-center">
    <h1 class="color-violet grow">AST Traversal</h1>
    <p class="text-right"><code>Macro.prewalk/2</code></p>
</div>

```elixir
@doc """
Performs a depth-first, pre-order traversal of quoted expressions.

Returns a new AST where each node is the result of invoking `fun` on each
corresponding node of `ast`.
"""
@spec prewalk(t, (t -> t)) :: t
```

<v-click>

````md magic-move
```elixir
fn node -> IO.inspect(node) end
```

```elixir
Macro.prewalk(ast, fn node -> IO.inspect(node) end)
```

```elixir
Macro.prewalk(Code.string_to_quoted!(code), fn node -> IO.inspect(node) end)
```

```elixir
"""
x + y
"""
|> Code.string_to_quoted!()
|> Macro.prewalk(fn node -> IO.inspect(node) end)
```

```elixir
"""
x + y
"""
|> Code.string_to_quoted!()
|> Macro.prewalk(fn node -> IO.inspect(node) end)

# `IO.inspect/1` calls
{:+, [line: 1], [{:x, [line: 1], nil}, {:y, [line: 1], nil}]}
{:x, [line: 1], nil}
{:y, [line: 1], nil}
```

```elixir
"""
x + y
"""
|> Code.string_to_quoted!()
|> Macro.prewalk(fn node -> IO.inspect(node) end)

# `IO.inspect/1` calls
{:+, [line: 1], [{:x, [line: 1], nil}, {:y, [line: 1], nil}]}
{:x, [line: 1], nil}
{:y, [line: 1], nil}

# Output
{:+, [line: 1], [{:x, [line: 1], nil}, {:y, [line: 1], nil}]}
```
````

</v-click>

<style>
div > p > code {
    @apply color-violet;
    @apply bg-violet-100;
}
</style>

---
layout: two-cols-title
---

::title::

<div class="flex w-full items-center">
    <h1 class="color-violet grow">AST Traversal</h1>
    <p class="color-gray text-right">Unsafe String To Atom</p>
</div>

::left::

````md magic-move {at:2}
```elixir
Code.string_to_quoted!("""
String.to_atom("ok")
""")
```
```elixir
fn node -> node end
```
```elixir
fn
  {:., _, _} = node ->
    node

  node ->
     node
end
```
```elixir
fn
  {:., _, [
    {:__aliases__, _, [:String]},
    :to_atom
  ]} = node ->
    IO.puts("Call Detected: String.to_atom/1 .")
    node

  node ->
    node
end
```
```elixir
checker = fn
  {:., _, [
    {:__aliases__, _, [:String]},
    :to_atom
  ]} = node ->
    IO.puts("Call Detected: String.to_atom/1 .")
    node

  node ->
    node
end

"""
String.to_atom("ok")
"""
|> Code.string_to_quoted!()
|> Macro.prewalk(checker)
```
````

::right::

```elixir {all|2-9}{at:1}
{
  {
    :.,
    [line: 1],
    [
      {:__aliases__, [line: 1], [:String]},
      :to_atom
    ]
  },
  [line: 1],
  ["ok"]
}
```

<!--
- Let's see the AST for a call to `String.to_atom/1`
- Knowing the "shape" of the node allows us to pattern-match against it
- The `:.` node represents a remote function call or dot call.
- With this information in mind, we can leverage `IO.puts/1` to print a message
when a call to `String.to_atom/1` is detected.
-->

---
layout: section
color: violet
---

# Credo

#### A static code analysis tool for the Elixir language with a focus on code consistency and teaching.

<div class="absolute bottom-8 left-0 w-full text-xl flex flex-col items-center">
    <a href="https://github.com/rrrene/credo" target="_blank">
        <carbon-logo-github class="text-xl"/> rrrene/credo
    </a>
</div>

<style>
a {
    @apply color-violet-300;
    border-bottom: none;
}

a:hover {
    border-bottom: none;
    @apply color-violet-100;
}

h4 {
    font-family: 'Inter';
    font-weight: 300;
}
</style>

---
layout: default
---

<div class="flex w-full items-center">
    <h1 class="color-violet grow">Credo</h1>
    <p class="color-gray text-right">Custom Check</p>
</div>

````md magic-move
```elixir
defmodule Credo.Check do
  @moduledoc """
  `Check` modules represent the checks which are run during Credo's analysis.
  """

  @callback run(source_file :: Credo.SourceFile.t(), params :: Keyword.t()) :: list(Credo.Issue.t())
end
```
```elixir {1-7}
defmodule Credo.SourceFile do
  @moduledoc """
  `SourceFile` structs represent a source file in the codebase.
  """

  @type t :: %__MODULE__{filename: nil | String.t(), hash: String.t(), status: :valid | :invalid | :timed_out}
end

defmodule Credo.Check do
  @callback run(source_file :: Credo.SourceFile.t(), params :: Keyword.t()) :: list(Credo.Issue.t())
end
```
```elixir {1-11}
defmodule Credo.Issue do
  @moduledoc """
  `Issue` structs represent all issues found during the code analysis.
  """

  defstruct message: nil,
            check: nil,
            filename: nil,
            line_no: nil,
            ...
end

defmodule Credo.SourceFile do
  @type t :: %__MODULE__{filename: nil | String.t(), hash: String.t(), status: :valid | :invalid | :timed_out}
end

defmodule Credo.Check do
  @callback run(source_file :: Credo.SourceFile.t(), params :: Keyword.t()) :: list(Credo.Issue.t())
end
```
```elixir
defmodule Credo.Issue do
  @moduledoc """
  `Issue` structs represent all issues found during the code analysis.
  """

  defstruct message: nil,
            check: nil,
            filename: nil,
            line_no: nil,
            ...
end

defmodule Credo.SourceFile do
  @type t :: %__MODULE__{filename: nil | String.t(), hash: String.t(), status: :valid | :invalid | :timed_out}
end

defmodule Credo.Check do
  @callback run(source_file :: Credo.SourceFile.t(), params :: Keyword.t()) :: list(Credo.Issue.t())
end
```
````

<!--
- Writing a custom check involves using `Credo.Check` and implementing the
`run/2` function
- The `run/2` function receives a `Credo.SourceFile` and is expected to return a
list of `Credo.Issue` structs
-->

---
layout: default
---

<div class="flex w-full items-center">
    <h1 class="color-violet grow">Credo</h1>
    <p class="color-gray text-right">Unsafe String To Atom</p>
</div>

````md magic-move
```elixir
defmodule Credo.Check do
  @moduledoc """
  `Check` modules represent the checks which are run during Credo's analysis.
  """

  @callback run(
    source_file :: Credo.SourceFile.t(),
    params :: Keyword.t()
  ) :: list(Credo.Issue.t())
end
```
```elixir
defmodule Credo.Check.Warning.UnsafeToAtom do
  use Credo.Check
end
```
```elixir
defmodule Credo.Check.Warning.UnsafeToAtom do
  use Credo.Check

  @impl true
  def run(%SourceFile{} = source_file, params) do
    issue_meta = IssueMeta.for(source_file, params)
    Credo.Code.prewalk(source_file, &traverse(&1, &2, issue_meta))
  end
end
```
```elixir
defmodule Credo.Check.Warning.UnsafeToAtom do
  use Credo.Check

  @impl true
  def run(%SourceFile{} = source_file, params) do
    issue_meta = IssueMeta.for(source_file, params)
    Credo.Code.prewalk(source_file, &traverse(&1, &2, issue_meta))
  end

  defp traverse({{:., _loc, call}, meta, args} = ast, issues, issue_meta) do
    case get_forbidden_call(call, args) do
      {bad, suggestion, trigger} ->
        {ast, issues_for_call(bad, suggestion, trigger, meta, issue_meta, issues)}

      nil ->
        {ast, issues}
    end
  end
end
```
```elixir
defmodule Credo.Check.Warning.UnsafeToAtom do
  use Credo.Check, ...

  @impl true
  def run(%SourceFile{} = source_file, params) do
    issue_meta = IssueMeta.for(source_file, params)
    Credo.Code.prewalk(source_file, &traverse(&1, &2, issue_meta))
  end

  defp traverse({{:., _loc, call}, meta, args} = ast, issues, issue_meta) do
    case get_forbidden_call(call, args) do
      {bad, suggestion, trigger} ->
        {ast, issues_for_call(bad, suggestion, trigger, meta, issue_meta, issues)}

      nil ->
        {ast, issues}
    end
  end

  defp get_forbidden_call([{:__aliases__, _, [:String]}, :to_atom], [_]) do
    {"String.to_atom/1", "String.to_existing_atom/1", "String.to_atom"}
  end
end
```
```elixir
defmodule Credo.Check.Warning.UnsafeToAtom do
  use Credo.Check, ...

  @impl true
  def run(%SourceFile{} = source_file, params) do
    issue_meta = IssueMeta.for(source_file, params)
    Credo.Code.prewalk(source_file, &traverse(&1, &2, issue_meta))
  end

  defp traverse({{:., _loc, call}, meta, args} = ast, issues, issue_meta) do
    case get_forbidden_call(call, args) do
      {bad, suggestion, trigger} ->
        {ast, issues_for_call(bad, suggestion, trigger, meta, issue_meta, issues)}

      nil ->
        {ast, issues}
    end
  end

  defp get_forbidden_call([{:__aliases__, _, [:String]}, :to_atom], [_]), do: ...
  defp get_forbidden_call([{:__aliases__, _, [:List]}, :to_atom], [_]), do: ...
  defp get_forbidden_call([:erlang, :binary_to_atom], [_, _]), do: ...
end
```
````

<!--
- A lot of the source code has been removed for clarity, seeing as this serves
only as an example to compare how Credo also leverages Elixir's AST to provide
its functionalities
- The `Credo.Code.prewalk/2` function can be used to traverse the AST of a
`Credo.SourceFile`. It's a light wrapper around `Code.prewalk/2`, but it first
handles converting the `Credo.SourceFile` struct to an AST.
- The `issues_for_call` function simply adds another `Credo.Issue` function to
the list of issues reported by this check, by leveraging `format_issue/3` with
the provided information, which builds a `Credo.Issue` struct.
-->

---
layout: side-title
color: violet
align: cm-lm
---

::title::

<div class="flex flex-col gap-4">
    <h1>AST Traversal</h1>
    <h3 style="font-family: 'Inter'; font-weight: 300">Transformations</h3>
</div>

::content::

<v-clicks>

- Leverage `Macro.prewalk/2` to traverse the AST
- Find calls to `String.to_atom/1`
- Replace with calls to `String.to_existing_atom/1`

</v-clicks>

<!--
- We can leverage `Macro.prewalk/2` again, modifying the node instead of simply
returning it.
- The call to `String.to_existing_atom/1` will raise an exception if the atom
does not exist.
-->

---
layout: two-cols-title
layoutClass: gap-4
---

::title::

<div class="flex w-full items-center">
    <h1 class="color-violet grow">AST Traversal</h1>
    <p class="color-gray text-right">String To Atom Transformation</p>
</div>

::left::

````md magic-move
```elixir
Code.string_to_quoted!("""
String.to_atom("ok")
""")
```
```elixir
fn node -> node end
```
```elixir
fn
  {:., _, _} = node ->
    node

  node ->
     node
end
```
```elixir
fn
  {:., call_meta, [
    {:__aliases__, alias_meta, [:String]},
    :to_atom
  ]} = node ->
    {:., call_meta, [
      {:__aliases__, alias_meta, [:String]},
      :to_existing_atom
    ]}

  node ->
    node
end
```
```elixir {4,8}
fn
  {:., call_meta, [
    {:__aliases__, alias_meta, [:String]},
    :to_atom
  ]} = node ->
    {:., call_meta, [
      {:__aliases__, alias_meta, [:String]},
      :to_existing_atom
    ]}

  node ->
    node
end
```
```elixir
processor = fn
  {:., call_meta, [
    {:__aliases__, alias_meta, [:String]},
    :to_atom
  ]} = node ->
    {:., call_meta, [
      {:__aliases__, alias_meta, [:String]},
      :to_existing_atom
    ]}

  node ->
    node
end

"""
String.to_atom("ok")
"""
|> Code.string_to_quoted!()
|> Macro.prewalk(processor)
```
```elixir
processor = fn
  {:., call_meta, [
    {:__aliases__, alias_meta, [:String]},
    :to_atom
  ]} = node ->
    {:., call_meta, [
      {:__aliases__, alias_meta, [:String]},
      :to_existing_atom
    ]}

  node ->
    node
end

"""
String.to_atom("ok")
"""
|> Code.string_to_quoted!()
|> Macro.prewalk(processor)
```
```elixir {20}
processor = fn
  {:., call_meta, [
    {:__aliases__, alias_meta, [:String]},
    :to_atom
  ]} = node ->
    {:., call_meta, [
      {:__aliases__, alias_meta, [:String]},
      :to_existing_atom
    ]}

  node ->
    node
end

"""
String.to_atom("ok")
"""
|> Code.string_to_quoted!()
|> Macro.prewalk(processor)
|> Macro.to_string()
```
````

::right::

````md magic-move {at:6}
```elixir
{
  {
    :.,
    [line: 1],
    [
      {:__aliases__, [line: 1], [:String]},
      :to_atom
    ]
  },
  [line: 1],
  ["ok"]
}
```
```elixir
{
  {
    :.,
    [line: 1],
    [
      {:__aliases__, [line: 1], [:String]},
      :to_existing_atom
    ]
  },
  [line: 1],
  ["ok"]
}
```
```elixir
String.to_existing_atom("ok")
```
````

<!--
- Revisiting the AST for a call to `String.to_atom/1`
- We can, once again, pattern-match against the specific node that performs the
function call
- Since we only need to change the function being called, we simply replace
`:to_atom` with `:to_existing_atom`
- This will update the resulting AST
- We can then leverage `Macro.to_string/1` to conver the AST back to Elixir code
-->

---
layout: section
color: violet
---

# Sourceror

#### Utilities to manipulate Elixir source code

<div class="absolute bottom-8 left-0 w-full text-xl flex flex-col items-center">
    <a href="https://github.com/doorgan/sourceror" target="_blank">
        <carbon-logo-github class="text-xl"/> doorgan/sourceror
    </a>
</div>

<style>
a {
    @apply color-violet-300;
    border-bottom: none;
}

a:hover {
    border-bottom: none;
    @apply color-violet-100;
}

h4 {
    font-family: 'Inter';
    font-weight: 300;
}
</style>

<!--
- Sourceror builds on top of Elixir's AST
- It introduces its own custom format, that is still compatible with Elixir's
standard library, but that can be leveraged by Sourceror's function
implementations
-->

---
layout: side-title
color: violet
align: cm-lm
---

::title::

# Sourceror

::content::

<v-clicks>

- Comments – `Code.string_to_quoted_with_comments/1` introduces extra structure
- Zippers – Easier navigation
- Patches – Range-based edits that preserve original formatting

</v-clicks>

<!--
- Sourceror builds on top of Elixir's AST and it's a tool you'd reach out to in
more special cases where you either need to preserve comments, easier navigation
of the full AST or non-destructive edits
- It introduces its own custom format, that is still compatible with Elixir's
standard library, but that can be leveraged by Sourceror's function
implementations
- Souceror's Zippers offer a zipper data structure that makes it easier to
navigate the AST, as it keeps track of the full context of where the user is in
the node, allowing one to navigate to the parent node or children node very
easily, something that is not so easy with something like `Macro.prewalk/3`
-->

---
layout: two-cols-title
---

::title::

<div class="flex w-full items-center">
    <h1 class="color-violet grow">Sourceror</h1>
    <p class="color-gray text-right">Code With Comments</p>
</div>


::left::

````md magic-move {at:1}
```elixir
Code.string_to_quoted_with_comments!("""
# Should we replace this call?
String.to_atom("ok")
""")
```
```elixir {all|2-9|10-18|all}
{
  {
    {:., [line: 2], [
      {:__aliases__, [line: 2], [:String]},
      :to_atom
    ]},
    [line: 2],
    ["ok"]
  },
  [
    %{
      line: 1,
      text: "# Should we replace this call?",
      column: 1,
      next_eol_count: 1,
      previous_eol_count: 1
    }
  ]
}
```
```elixir
"""
# Should we replace this call?
String.to_atom("ok")
"""
|> Code.string_to_quoted!()
|> Macro.to_string()
```
```elixir
String.to_atom("ok")
```
````

::right::

````md magic-move {at:1}
```elixir
Sourceror.parse_string!("""
# Should we replace this call?
String.to_atom("ok")
""")
```
```elixir {all|all|all|6-21}
{
  {:., _, [
    {:__aliases__, _, [:String]},
    :to_atom
  ]},
  [
    trailing_comments: [],
    leading_comments: [
      %{
        line: 1,
        text: "# Should we replace this call?",
        column: 1,
        next_eol_count: 1,
        previous_eol_count: 1
      }
    ],
    end_of_expression: [newlines: 1, line: 2, column: 21],
    closing: [line: 2, column: 20],
    line: 2,
    column: 8
  ],
  [ {:__block__, _, ["ok"]} ]
}
```
```elixir
"""
# Should we replace this call?
String.to_atom("ok")
"""
|> Sourceror.parse_string!()
|> Sourceror.to_string()
```
```elixir
# Should we replace this call?
String.to_atom("ok")
```
````

<!--
- Using `Code.string_to_quoted_with_comments/1` returns a two element tuple,
with the first element being the AST, while the second is just the list of
comments.
- On the other hand, `Sourceror.parse_string!/1` returns a structure compatible
with Elixir's AST, while also ensuring that the comments are preserved in the
node's metadata.
- Using `Sourceror.to_string/1` will correctly handle the comment metadata,
ensuring that the comments are added back to the generated code, while using
`Code.string_to_quoted` followed by `Macro.to_string` will remove any comments.
-->

---
layout: side-title
color: violet
align: cm-lm
---

::title::

# Conclusion

::content::

<v-clicks>

- AST enables powerful code analysis and transformation capabilities
- Tools like Credo and Sourceror build upon Elixir's AST foundations
- Understanding AST patterns unlocks advanced metaprogramming techniques

</v-clicks>

---
layout: section
color: violet
---

<h1 class="text-center">Thanks!</h1>

<div class="absolute bottom-8 left-0 w-full text-xl flex flex-col items-center">
    <a href="https://github.com/dinocosta" target="_blank">
        <carbon-logo-github class="text-xl"/> dinocosta
    </a>
    <a href="https://twitter.com/dinocosta_" target="_blank" class="border-">
        <carbon-logo-twitter class="text-xl"/> dinocosta_
    </a>
</div>

<style>
a {
    @apply color-violet-300;
    border-bottom: none;
}

a:hover {
    border-bottom: none;
    @apply color-violet-100;
}
</style>

<!--
**How to avoid false positives?**
Use `Macro.expand/1` in order to understand if the `:__aliases__` node is
actually a call to `Elixir.String` and not some other module.

**When to use `Macro.prewalk/2` vs `Macro.postwalk/2`?**
`Macro.postwalk/2` can help when your transformation requires recursive function
calls, otherwise you might end up in an infinite loop.

**How to integrate in an existing codebase?**
- For static code analysis, just use Credo.
- For code transformation, possibly create a new mix task and either leverage
the standard library or Sourceror.
-->
