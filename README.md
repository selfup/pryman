# pryman
Ways to snapshot pry session. Can only save 100 lines at a time.

# How it works

Paste this into your **".pryrc"** file:

```ruby
Pry::Commands.block_command "batman", "Save the session" do |filename|
  filename ||= "batman.rb"
  filename = Pathname.new filename

  while File.exist? filename
    no_ext      = filename.basename.sub_ext('').to_s
    counter     = no_ext[/\d+$/].to_i.next
    incremented = Pathname.new(no_ext.sub /\d*$/, counter.to_s)
    filename    = filename.dirname + incremented.sub_ext(filename.extname)
  end

  inout = _pry_.input_array.to_a.zip(_pry_.output_array.to_a)
  body  = inout.map { |input, output|
    inspected_output = output.inspect.gsub(/^/, '  # ')
    "#{input}#{inspected_output}"
  }.join("\n")
  File.write(filename, body)
end
```

Say you are learning a new concept and testing it out in pry, but you want to save it to a **".rb"** file.

Now you just type in (while in pry):

    $ batman

It will write/ovewrite a file called **"batman.rb""**.

If you run **"batman"** again, it will add **"1"** to **"batman"**. So **"batman1.rb"**.

If you call: 

    $ batman lol.rb

It will now make a file called **"lol.rb"** instead of batman.

**It will save the new file to your CURRENT WORKING directory**

----------------------------------------------------------------------------------------

Excited to Start Turing on MONDAY!

# Many Thanks to Josh Cheek from Turing for helping me out.

