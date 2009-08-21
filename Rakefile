# -*- ruby -*-

require 'rubygems'
require 'hoe'

Hoe.spec('linebreak') do
  developer('Alexander E. Fischer', 'aef@raxys.net')

  self.rubyforge_name = 'linebreak'
  self.extra_dev_deps = %w{user-choices rspec popen4}
  self.url = 'https://rubyforge.org/projects/aef/'
  self.readme_file = 'README.rdoc'
  self.extra_rdoc_files = %w{README.rdoc}
  self.spec_extras = {
    :rdoc_options => ['--main', 'README.rdoc', '--inline-source', '--line-numbers', '--title', 'Linebreak']
  }
  self.rspec_options = ['--options', 'spec/spec.opts']
  self.remote_rdoc_dir = ''
end

# vim: syntax=Ruby
