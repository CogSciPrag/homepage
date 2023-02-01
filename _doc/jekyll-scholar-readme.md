# Setting up Jekyll-scholar with github-pages

The following instructions presuppose that you already have a functioning website buil using Jekyll and github-pages.

## Step 1: Include jekyll-scholar as plugin

To add Jekyll-scholar to your website, you must update the Gemfile (*Gemfile*) and the configuration file (*_config.yml*) in your main website repository.

**Changes to Gemfile**

Add the following line of code:
	
	gem 'jekyll-scholar', group: jekyll_plugins

**Changes to configuration file**

Add the following lines of code:

	plugins:
		- jekyll-scholar
     
  
## Step 2: Set up github action

For security reasons, github does not, by default, allow the loading of just any kind of Jekyll plugin when building your website. The solution is to set up a continuous integration pipeline to build the site on each push and publish the build output on the master branch. GitHub Actions allow us to do just that. We define our own build action in the following steps:

**1. Generate a new source branch**: Generate a branch called *source* to which you move all files currently located on the main/master branch.

**2. Add your new Github action**: On your *source* branch, create a new folder called *.github/workflows*. Within that folder, create a new file called *build.yml*.
Copy the contents of this file (https://github.com/CogSciPrag/homepage/blob/source/.github/workflows/build.yml) into it.
You may have to adapt this code by updating the Ruby version (line 19), switching from *main* to *master* branch, or other.

## Step 3: Configure citations with Jekyll-scholar

By default, Jekyll-scholar expects to find a bibliography file called *references.bib* in a *_bibliography* folder of your source branch. If necessary, we can override these settings by changing the plugin settings in the *_config.yml* file (see the readme at https://github.com/inukshuk/jekyll-scholar). For now, we will just keep it like that. Thus...

**Add your .bib file**: Create a new folder *_bibliography* containing a file *references.bib*. From now on, you can use Jekyll-scholar commands as indicated below.

# Example usage of Jekyll-scholar

In the following, I will demonstrate basic commands of the Jekyll-scholar plugin. For details, please see the readme on https://github.com/inukshuk/jekyll-scholar

## Basic setup

I have included a *.bib* file in the *source* branch of the website repository (see above for details). Jekyll-scholar also supports the use of several *.bib* files; in that case the scholar plugin settings in the *_config.yml* file need to be updated accordingly or the *.bib* file you aree citing from needs to be referenced in the cite command.

A github action ensures that the website build will be automatically updated on every push to the *source* branch. Thus, if you modify a *.bib* file whose entries are printed as a bibliography somewhere on the website, the html generated from the *.bib* build automatically updates along with it. Jekyll-scholar also allows for the use of in-text citations and formatted references, cross-references to other pages on the wbsite and more using commands similar to LaTeX citation commands.

### In-text citations

To cite entries from the source *.bib* file, you can use the following base command:

	{% cite bibentry %}

This will result in a citation like this: (Schwab & Liu, 2020)

You can cite multiple items in a single citation by referencing all ids of the items you wish to quote separated by spaces. For example, *{% cite example1 example2 %}* would produce something like this:

(Schwab & Liu, 2020; Schwab & Liu, 2022)

Suppressing author names, adding page numbers, ..., are possible with additional specifiers in the cite command. See the readme file linked above.

### Formatted references

To print a full formatted reference (as we may wish to do to display all lab publications), use the *reference* tag:

	{% reference bibentry %}

This will produce a full bibliography entry, e.g.:

Schwab, J., & Liu, M. (2022). Processing Attenuating NPIs in Indicative and Counterfactual Conditionals. Frontiers in Psychology, 13, 3083. https://doi.org/10.3389/fpsyg.2022.894396

### Bibliography

The full bibliography file, or all cited entries, can be printed with the *bibliography* command. Bibliography and citation styles can be changed with user-specified *.css* files.

	{% bibliography --cited %}

Schwab, J., & Liu, M. (2020). Lexical and contextual cue effects in discourse expectations: Experimenting with German ’zwar...aber’ and English ’true/sure...but.’ Dialogue & Discourse, 11(2), 74–109. https://doi.org/10.5087/dad.2020.203

Schwab, J., & Liu, M. (2022). Processing Attenuating NPIs in Indicative and Counterfactual Conditionals. Frontiers in Psychology, 13, 3083. https://doi.org/10.3389/fpsyg.2022.894396

### Open issues (29/01/2023)

To change the style of references or bibliography entries (e.g., it would be nice to have clickable urls or optionally include references to data repositories), I will need to make some hacky changes to the *.cls* files. It should be possible, though.

In the above examples, I am using the *APA citation format*. Other formats are possible, we only need to settle on one. 
