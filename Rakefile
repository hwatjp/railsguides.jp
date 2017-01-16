task default: 'assets:precompile'

namespace :assets do
  task :precompile do
    Rake::Task['clean'].invoke

    load './guides/Rakefile'
    Dir.chdir('./guides') do
      Rake::Task['gtt:allocate'].invoke
      Rake::Task['gtt:replacer'].invoke
      Rake::Task['guides:clean'].invoke

      ENV['GUIDES_LANGUAGE'] = 'ja'
      Rake::Task['guides:generate:html'].invoke('--trace')
      Rake::Task['guides:tanpopo'].invoke
    end

    sh 'JEKYLL_ENV=production bundle exec jekyll build'
  end
end

# cf. How to test a Jekyll site
# http://joenyland.me/blog/how_to_test_a_jekyll_site/
require 'html/proofer'
task test: [:build] do
  HTML::Proofer.new('./_site', {
                      check_opengraph: true,
                      check_favicon: true,
                      check_html: true,
                      disable_external: true,
                      file_ignore: %w(),
                      url_ignore:  %w(),
                      http_status_ignore: [0, 500, 999],
                    }).run
end

task build: [:clean] do
  system 'bundle exec jekyll build'
end

task :clean do
  system 'bundle exec jekyll clean'
end
