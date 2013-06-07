desc "Install git hooks"
task :install_hooks do
  Dir["hooks/*"].each do |path|
    file = File.expand_path(path)
    link = File.expand_path(".git/#{path}")

    unless File.symlink?(link)
      File.symlink(file, link)
    end
  end
end
