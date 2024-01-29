---
layout: post
title: vim-polyglot / coc.nvim
date: 2022-04-26 06:02 +0900
category:
- 개발
- 도구
tags:
- vim
---

2년 전의 깨달음을 떠올리며 정리해본다.

Neovim 이나 SpaceVim 같이 미리 정제되고 커스터마이징 된 플러그인들을 기능으로 탑재하여 관리하는 vim-fork 부류를 사용하지 않는 한, vim 은 여러가지 잡다한 설정들을 스스로 해야하고, 설정과 관련된 난관을 겪을 수 있다.

개인적으로는 순정품을 좋아해서(...) vim 을 터미널에서 사용하고 있다. 사실 tmux 와의 궁합 때문인데, 익숙해지다 보니까 굳이 다른 편집기와 터미널의 조합으로 손길이 가질 않더라. 설정을 하나하나 추가하고 바꾸는 것이 입맞에 맞추는 과정이라 재미있기도 하다. 시간만 허락한다면.

## **vim-polyglot**
그런 재미들 중에서 제외하고 싶은게 있었으니… 그것은 프로그래밍 언어 지원(language support) 이다. 

문법 강조나 포맷, 들여쓰기 혹은 Syntastic 과 같은 외부 문법 체커와의 통합에도 관련된 기능인데, 딱히 설정이 까다롭다기 보다는 .vimrc 내부 플러그인 목록의 덩치를 키우고, 언어에 따라서 기능이 부분적으로 겹치는 것들도 있어서 조금 성가신 편이다.

vim-fork 들은 기본적으로 통합해서 제공하지만, 내가 사용중이었던 여러 플러그인들 중 언어 지원관련은 이런 모양이었다:
```vim
Plugin 'pangloss/vim-javascript'
Plugin 'kchmck/vim-coffee-script'
Plugin 'chemzqm/vim-jsx-improve'
Plugin 'leafgarland/typescript-vim'
Plugin 'peitalin/vim-jsx-typescript'
Plugin 'cakebaker/scss-syntax.vim'
Plugin 'gcorne/vim-sass-lint'
Plugin 'rust-lang/rust.vim'
Plugin 'racer-rust/vim-racer'
Plugin 'elixir-lang/vim-elixir'
Plugin 'udalov/kotlin-vim'  
```
{: file="~/.vimrc" }

Rust 관련 플러그인들을 찾다가 [vim-polyglot](https://github.com/sheerun/vim-polyglot) 을 우연히 발견하게 되었는데, 거의 대부분의 주류 언어들을 통합 지원하면서도 vim 의 실행시간이나 설치/업데이트 면에서 강점이 있어서 설치를 했다.

```vim
Plugin 'sheerun/vim-polyglot'
```
{: file="~/.vimrc" }

이렇게 한 줄로 끝이다. 참 쉽다. (참고로 플러그인들은 [Vundle](https://github.com/VundleVim/Vundle.vim) 로 관리하고 있다.)

## **coc.nvim**
언어 지원은 코딩 시 표면적/시각적으로 도움을 주는 반면, LSP(Language Server Protocol)는 코드 완성이나 타입 체킹, 정의된 곳으로 이동 등을 통해서 조금 더 코딩을 편하고 정확하게 할 수 있는 기능을 제공한다.

비교하자면 Visual Studio 의 Intellisense 같은 기능을 수행하는 것인데, 수 많은 언어들에 따라 해당 기능을 각각 개발하려면 편집기의 크기가 커지고 복잡해지기 때문에, 개발 커뮤니티에 의해 서버(언어별로 편의에 관련된 기능을 제공)-클라이언트(기능을 사용하는 도구) 방식으로 LSP 가 탄생하게 되었다.

기존에는 코드 완성기능을 위해 [YouCompleteMe](https://github.com/ycm-core/YouCompleteMe#lsp-configuration) (YCM) 를 사용했다. YCM 의 설정 중 [ALE](https://github.com/dense-analysis/ale) 을 LSP 클라이언트로 지정해서 썼고. 그런데 문제는, 딱히 싫지는 않은데 마음에 들지도 않는다는 것이었다.

![](https://res.cloudinary.com/leonkim/image/upload/v1650927269/blog/2022-04-26-vim-polyglot-coc-nvim/1_gxqeak.png)
_이렇게 밑줄 뿐만이 아닌, 조금 더 눈에 띄는 경고와 친절한 설명이 보고 싶었다._

\
제대로 된 설정을 하지 않았기에 YCM 과 ALE 두 플러그인을 함께 쓰는 것에 좋은 인상을 받지 못했던 것 같다. 결정적인 원인은 두 가지 였는데, 타입의 참조문서를 원할때 바로 띄워서 보고 싶었고, 경고/에러등을 command line 이 아닌 곳에서 한 줄 이상으로 보고 싶었다. 그것들이 가장 불편했다.

그 이후 이것저것 사용을 시도해봤고, 정착하게 된 것이 [coc.nvim](https://github.com/neoclide/coc.nvim). 앞서 언급한 두 플러그인의 기능을 통합적으로 사용할 수 있는 것이 가장 매력적이었다. 관련 `.vimrc`는 편의에 따라 이렇게 설정했는데, 커서 위에서 `Shift + T`를 입력해서 원할때만 해당 관련 문서를 볼 수 있도록 해놓은, hover documentation 이라고 이름 붙은 기능을 사용하고 있다.

```vim
let g:coc_global_extensions = ['coc-tsserver', 'coc-json', 'coc-rust-analyzer', 'coc-graphql', 'coc-prettier']

" Use tab for trigger completion with characters ahead and navigate.
inoremap <silent><expr> <TAB>
      \ pumvisible() ? "\<C-n>" :
      \ <SID>check_back_space() ? "\<TAB>" :
      \ coc#refresh()
inoremap <expr><S-TAB> pumvisible() ? "\<C-p>" : "\<C-h>"

function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~# '\s'
endfunction

" Use <c-space> to trigger completion.
inoremap <silent><expr> <c-space> coc#refresh()
" Remap keys for applying codeAction to the current line.
nmap <leader>ac  <Plug>(coc-codeaction)
" Apply AutoFix to problem on the current line.
nmap <leader>qf  <Plug>(coc-fix-current)

" GoTo code navigation.
nmap <silent> gd <Plug>(coc-definition)
nmap <silent> gy <Plug>(coc-type-definition)
nmap <silent> gi <Plug>(coc-implementation)
nmap <silent> gr <Plug>(coc-references)

" Hover documentation
nnoremap <silent> K :call <SID>show_documentation()<CR>
function! s:show_documentation()
  if (index(['vim','help'], &filetype) >= 0)
    execute 'h '.expand('<cword>')
  else
    call CocAction('doHover')
  endif
endfunction

" Prettier (coc-prettier)
vmap <leader>p  <Plug>(coc-format-selected)
nmap <leader>p  <Plug>(coc-format-selected)

" Jump to next/previous problem
nmap <silent> <Leader>j <Plug>(coc-diagnostic-next-error)
nmap <silent> <Leader>k <Plug>(coc-diagnostic-prev-error)
```
{: file="~/.vimrc"}

\
추가로 CoC 전용 설정 파일을 생성해야 한다. 요즘 프로젝트에서 주로 쓰는 TypeScript 관련 설정은 이렇게 해두었다: 

```json
{
  ...
  "coc.preferences.formatOnType": true,
  "coc.preferences.jumpCommand": "tabe",
  "codeLens.enable": true,
  "diagnostic.errorSign": "XX",
  "diagnostic.warningSign": "!!",
  "diagnostic.infoSign": "ii",
  "diagnostic.hintSign": "??",
  "diagnostic.enableHighlightLineNumber": true,
  "signature.enable": true,
  "signature.target": "echo",
  "tsserver.npm": "/usr/local/bin/npm",
  "typescript.format.enabled": false
  ...
}
```
{: file="~/.vim/coc-settings.json"}

\
결과는 마음에 들었다. 아래 스크린 샷으로 확인할 수 있는 결과 이외에도 경고/에러표시와 자동 수정기능 등 다양한 기능들을 사용할 수 있다. 
![](https://res.cloudinary.com/leonkim/image/upload/v1650927194/blog/2022-04-26-vim-polyglot-coc-nvim/2_ywwd8v.png)
_코드 자동완성_

![](https://res.cloudinary.com/leonkim/image/upload/v1650931308/blog/2022-04-26-vim-polyglot-coc-nvim/Screen_Shot_2022-04-26_at_9.01.36_AM_gjmd1f.png)
_Shift+K 를 통한 관련 참조문서 열람_

\
**vim 은 평생 배우는 것**이라는 말은 사용해보면 와닿게 되는 것 같다. 그래서 궁금하더라도 조급해 하지 않고 천천히 배우면서 ~~도 닦은 기분으로~~ 쓰는게 핵심인 것 같고. (그래서 잘 몰라도 계속 뻔뻔하게 사용하고 있다.)
