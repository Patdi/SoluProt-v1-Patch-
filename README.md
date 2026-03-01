<h1>SoluProt-v1-Patch</h1>

<p>
  Minor patch for compatibility with <strong>USEARCH 12</strong> and setup hints for <strong>TMHMM</strong>.
</p>

<hr />

<h2>Overview</h2>

<p>
  This patch documents two common compatibility issues when running
  <strong>SoluProt</strong> on modern Linux/WSL environments:
</p>

<ol>
  <li>
    <strong>USEARCH 12 compatibility</strong> (command-name mismatch with older SoluProt code)
  </li>
  <li>
    <strong>TMHMM Perl shebang fixes</strong> (old hardcoded Perl path in TMHMM scripts)
  </li>
</ol>

<hr />

<h2>References</h2>

<ul>
  <li>
    <strong>USEARCH 12</strong>:
    <a href="https://github.com/rcedgar/usearch12">https://github.com/rcedgar/usearch12</a>
  </li>
  <li>
    <strong>SoluProt</strong>:
    <a href="https://loschmidt.chemi.muni.cz/soluprot/?page=download">
      https://loschmidt.chemi.muni.cz/soluprot/?page=download
    </a>
  </li>
</ul>

<hr />

<h2>1) USEARCH-related changes (SoluProt compatibility with USEARCH 12)</h2>

<p>
  SoluProt uses an older USEARCH command name, which can cause failures when using
  <strong>USEARCH 12</strong>.
</p>

<p>
  In SoluProt's <code>_add_usearch_identity()</code> function, the command is built with:
</p>

<pre><code>-search_global</code></pre>

<p>
  For USEARCH 12, this may need to be changed to:
</p>

<pre><code>-usearch_global</code></pre>

<h3>Patch</h3>

<pre><code class="language-python"># Old (will fail with USEARCH 12)
usearch_arguments = ['-search_global', self.fasta_path,
                     '-db', self.pdb_db, '-id', '0.0', '-blast6out',
                     b6_path, '-threads', str(self.usearch_threads),
                     '-top_hits_only']

# New (USEARCH 12 compatible)
usearch_arguments = ['-usearch_global', self.fasta_path,
                     '-db', self.pdb_db, '-id', '0.0', '-blast6out',
                     b6_path, '-threads', str(self.usearch_threads),
                     '-top_hits_only']</code></pre>



<h2>2) TMHMM shebang fixes (Perl path compatibility)</h2>

<p>
  TMHMM scripts ship with an outdated Perl shebang:
</p>

<pre><code>#!/usr/local/bin/perl</code></pre>

<p>
  This path does not exist on modern Linux/WSL systems, causing SoluProt/TMHMM execution failures.
</p>

<p>
  Update TMHMM scripts to use:
</p>

<pre><code>#!/usr/bin/env perl</code></pre>

<h3>Files requiring fix</h3>
<ul>
  <li><code>tmhmm</code></li>
  <li><code>tmhmmformat.pl</code></li>
  <li><code>tmhmm.ORIG</code>code></li>
</ul>
<hr />


