require 'fileutils'

def tex_for_file(f)
    texc = "xelatex"
    opts = "-output-directory=_build -synctex=1 -interaction=nonstopmode"
    "#{texc} #{opts} #{f}"
end

main_files = {
    :english => "main_en.tex",
    :german => "main_de.tex"
}

task "default" => ["pre", "comp:en", "comp:de", "release", "clean"]

task "pre" do
    # Check existance of needed files
    main_files.each do |ver,fname|
        puts "Checking existance of #{fname}.."
        if File.exists? fname
            puts "Ok."
        else
            raise "#{fname} does not exist"
        end
    end

    # Check existance of _build folder
    puts "Checking existance of build folder (_build)"
    FileUtils.mkdir "_build" unless Dir.exist? "_build"
    puts "Ok."
end

namespace "comp" do
    desc "Compile the english version"
    task "en" do
        cmd = tex_for_file main_files[:english]
        system cmd
    end

    desc "Compile the german version"
    task "de" do
        cmd = tex_for_file main_files[:german]
        system cmd
    end

    desc "Compile all versions"
    task "all" do
        main_files.each do |_,fname|
            system (tex_for_file fname)
        end
    end
end

desc "Clean all generated files"
task "clean" do
    Dir.glob("_build/*").each do |f|
        File.delete f
    end
    #system "rm *.aux *.log *.out *.pdf"
end

desc "Release the CV"
task "release" do
    system "cp _build/*.pdf ."
end
