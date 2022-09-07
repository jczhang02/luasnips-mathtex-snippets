<h1 align="center">Luasnips MathTex Snippets :kissing_cat:</h1>

This repo contains mathtex snippets for `markdown` and `latex` filetypes.

Forked from [iurimateus](https://github.com/iurimateus/luasnip-latex-snippets.nvim), add `markdown` filetype support.

## Installation

```lua
use {
    "jczhang02/luasnips-mathtex-snippets",
	config = function()
		vim.cmd([[packadd LuaSnip]])
		vim.cmd([[packadd vimtex]])
		vim.cmd([[packadd vim-markdown]])
		require("luasnip-latex-snippets").setup()
		-- or setup({ use_treesitter = true })
	end,
	ft = { "tex", "markdown" },
}
use{
    "preservim/vim-markdown",
	opt = true,
	ft = "markdown",
	config = function()
		vim.cmd([[let g:vim_markdown_math = 1]])
	end,
}
```

**Notice: your luasnips config should set `enable_autosnippets = true`**

## Customization

An example file, located in `~/.config/nvim/luasnippets/tex.lua`:

```lua
--- self Latex snippets

local snips, autosnips = {}, {}

local tex = {}

tex.in_mathzone = function()
	return vim.fn["vimtex#syntax#in_mathzone"]() == 1
end

tex.in_comment = function()
	return vim.fn["vimtex#syntax#in_comment"]() == 1
end

tex.in_text = function()
	return not tex.in_mathzone() and not tex.in_comment()
end

tex.in_align = function()
	return vim.fn["vimtex#env#is_inside"]("align")[1] ~= 0
end

snips = {
	s(
		{ trig = "it", name = "italic", dscr = "Insert italic text." },
		{ t("\\textit{"), i(1), t("}") },
		{ condition = tex.in_text, show_condition = tex.in_text }
	),
	s(
		{ trig = "em", name = "emphasize", dscr = "Insert emphasize text." },
		{ t("\\emph{"), i(1), t("}") },
		{ condition = tex.in_text, show_condition = tex.in_text }
	),
}

autosnips = {
	s({
		trig = "([a-zA-Z])bf",
		name = "math bold",
		wordTrig = false,
		regTrig = true,
	}, {
		f(function(_, snip)
			return "\\mathbf{" .. snip.captures[1] .. "}"
		end, {}),
	}, { condition = tex.in_mathzone }),
	s(
		{ trig = "bf", name = "bold", dscr = "Insert bold text." },
		{ t("\\textbf{"), i(1), t("}") },
		{ condition = tex.in_text, show_condition = tex.in_text }
	),
	s(
		{ trig = "bf", name = "bold", dscr = "Insert bold text." },
		{ t("\\mathbf{"), i(1), t("}") },
		{ condition = tex.in_mathzone, show_condition = tex.in_text }
	),
}

return snips, autosnips
```

## Acknowledgement

[iurimateus/luasnip-latex-snippets](https://github.com/iurimateus/luasnip-latex-snippets.nvim)
