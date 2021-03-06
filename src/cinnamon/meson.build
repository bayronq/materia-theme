cinnamon_scss_depend_files = files([
  '../_color-palette.scss',
  '../_colors.scss',
  '../_variables.scss',
  '../gnome-shell/sass/_drawing.scss',
  '../gnome-shell/sass/_variables.scss',
  'sass/_common.scss',
  'sass/_extends.scss',
  'sass/_over-rides.scss',
])

foreach theme: themes
  cinnamon_dir = join_paths(theme['dir'], 'cinnamon')

  install_subdir(
    'assets',
    install_dir: cinnamon_dir,
  )

  install_data(
    'thumbnail.png',
    install_dir: cinnamon_dir,
  )

  #
  # SCSS
  #

  cinnamon_scss_conf = configuration_data()
  cinnamon_scss_conf.set('variant', theme['scss_variant'])
  cinnamon_scss_conf.set('topbar', theme['scss_topbar'])
  cinnamon_scss_conf.set('compact', theme['scss_compact'])

  # Configure SCSS file
  cinnamon_scss = configure_file(
    input: 'cinnamon.scss.in',
    output: 'cinnamon.@0@.scss'.format(theme['name']),
    configuration: cinnamon_scss_conf,
  )

  # Copy it from build dir to source dir
  run_command(
    'cp',
    cinnamon_scss,
    meson.current_source_dir(),
  )

  # Generate and install CSS file
  custom_target(
    'cinnamon.@0@.css'.format(theme['name']),
    input: 'cinnamon.@0@.scss'.format(theme['name']),
    output: 'cinnamon.@0@.css'.format(theme['name']),
    command: [sassc, sassc_opts, '@INPUT@', '@OUTPUT@'],
    depend_files: cinnamon_scss_depend_files,
    install: true,
    install_dir: cinnamon_dir,
  )

  # Rename it to a valid name
  meson.add_install_script(
    'mv',
    join_paths(get_option('prefix'), cinnamon_dir, 'cinnamon.@0@.css'.format(theme['name'])),
    join_paths(get_option('prefix'), cinnamon_dir, 'cinnamon.css'),
  )
endforeach
