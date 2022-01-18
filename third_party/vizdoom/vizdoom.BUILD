load("@envpool//third_party:common.bzl", "template_rule")

cc_binary(
    name = "arithchk",
    srcs = ["gdtoa/arithchk.c"],
)

genrule(
    name = "arith",
    srcs = [],
    outs = ["arith.h"],
    cmd = "$(execpath :arithchk) > $@",
    tools = [":arithchk"],
)

cc_binary(
    name = "qnan",
    srcs = [
        "gdtoa/qnan.c",
        ":arith.h",
    ],
)

genrule(
    name = "gd_qnan",
    srcs = [],
    outs = ["gd_qnan.h"],
    cmd = "$(execpath :qnan) > $@",
    tools = [":qnan"],
)

cc_library(
    name = "gdtoa",
    srcs = [
        "gdtoa/dmisc.c",
        "gdtoa/dtoa.c",
        "gdtoa/misc.c",
        ":arith.h",
        ":gd_qnan.h",
    ],
    hdrs = glob(["gdtoa/*.h"]),
    copts = [
        "-Wall",
        "-Wextra",
        "-DINFNAN_CHECK",
        "-DMULTIPLE_THREADS",
    ],
    strip_include_prefix = "gdtoa",
)

cc_library(
    name = "bzip2",
    srcs = [
        "bzip2/blocksort.c",
        "bzip2/bzlib.c",
        "bzip2/compress.c",
        "bzip2/crctable.c",
        "bzip2/decompress.c",
        "bzip2/huffman.c",
        "bzip2/randtable.c",
    ],
    hdrs = glob([
        "bzip2/*.h",
    ]),
    copts = [
        "-DBZ_NO_STDIO",
        "-Wall",
        "-Wextra",
        "-fomit-frame-pointer",
    ],
    strip_include_prefix = "bzip2",
)

template_rule(
    name = "viz_version",
    src = "src/viz_version.h.in",
    out = "viz_version.h",
    substitutions = {
        "@ViZDoom_VERSION_ID@": "1111",
        "@ViZDoom_VERSION_STR@": "1.1.11",
    },
)

cc_library(
    name = "lzma",
    srcs = glob(
        [
            "lzma/C/*.c",
        ],
        exclude = [
            "lzma/C/Threads.c",
            "lzma/C/LzFindMt.c",
        ],
    ),
    hdrs = glob(
        [
            "lzma/C/*.h",
        ],
        exclude = [
            "lzma/C/Threads.h",
            "lzma/C/LzFindMt.h",
        ],
    ),
    copts = [
        "-D_7ZIP_ST",
        "-Wall",
        "-Wextra",
        "-fomit-frame-pointer",
        "-D_7ZIP_PPMD_SUPPPORT",
    ],
    strip_include_prefix = "lzma/C",
)

cc_binary(
    name = "re2c",
    srcs = [
        "tools/re2c/actions.cc",
        "tools/re2c/code.cc",
        "tools/re2c/dfa.cc",
        "tools/re2c/main.cc",
        "tools/re2c/mbo_getopt.cc",
        "tools/re2c/parser.cc",
        "tools/re2c/scanner.cc",
        "tools/re2c/substr.cc",
        "tools/re2c/translate.cc",
    ] + glob(["tools/re2c/*.h"]),
    copts = [
        "-DHAVE_CONFIG_H",
    ],
)

cc_binary(
    name = "lemon",
    srcs = ["tools/lemon/lemon.c"],
)

genrule(
    name = "sc_man_scanner",
    srcs = ["src/sc_man_scanner.re"],
    outs = ["src/sc_man_scanner.h"],
    cmd = "$(execpath re2c) --no-generation-date -s -o $@ $<",
    tools = [":re2c"],
)

genrule(
    name = "lemon_deps",
    srcs = ["tools/lemon/lempar.c"],
    outs = ["lempar.c"],
    cmd = "cp $(SRCS) $(RULEDIR)",
)

genrule(
    name = "xlat_parser_y",
    srcs = ["src/xlat/xlat_parser.y"],
    outs = ["xlat_parser.y"],
    cmd = "cp $< $@",
)

genrule(
    name = "xlat_parser",
    srcs = [":xlat_parser_y"],
    outs = [
        "xlat_parser.c",
        "xlat_parser.h",
    ],
    cmd = "$(execpath lemon) $<",
    tools = [
        "lemon",
        ":lemon_deps",
        ":xlat_parser_y",
    ],
)

cc_library(
    name = "dumb",
    srcs = glob([
        "dumb/**/*.h",
        "dumb/**/*.c",
        "dumb/**/*.inc",
    ]),
    hdrs = glob([
        "dumb/include/*.h",
    ]),
    copts = [
        "-DNEED_ITOA=1",
        "-Wall",
        "-Wno-pointer-sign",
        "-Wno-uninitialized",
        "-Wno-unused-but-set-variable",
    ],
    includes = [
        "dumb/include",
    ],
)

cc_library(
    name = "gme",
    srcs = glob([
        "game-music-emu/gme/*.cpp",
        "game-music-emu/gme/*.h",
    ]),
    hdrs = [
        "game-music-emu/gme/gme.h",
    ],
    copts = [
        "-DLIBGME_VISIBILITY",
        "-fvisibility=hidden",
        "-fvisibility-inlines-hidden",
    ],
    strip_include_prefix = "game-music-emu",
)

cc_library(
    name = "headers",
    hdrs = [
        "src/g_doom/a_arachnotron.cpp",
        "src/g_doom/a_archvile.cpp",
        "src/g_doom/a_bossbrain.cpp",
        "src/g_doom/a_bruiser.cpp",
        "src/g_doom/a_cacodemon.cpp",
        "src/g_doom/a_cyberdemon.cpp",
        "src/g_doom/a_demon.cpp",
        "src/g_doom/a_doomimp.cpp",
        "src/g_doom/a_doomweaps.cpp",
        "src/g_doom/a_fatso.cpp",
        "src/g_doom/a_keen.cpp",
        "src/g_doom/a_lostsoul.cpp",
        "src/g_doom/a_painelemental.cpp",
        "src/g_doom/a_possessed.cpp",
        "src/g_doom/a_revenant.cpp",
        "src/g_doom/a_scriptedmarine.cpp",
        "src/g_doom/a_spidermaster.cpp",
        "src/g_heretic/a_chicken.cpp",
        "src/g_heretic/a_dsparil.cpp",
        "src/g_heretic/a_hereticartifacts.cpp",
        "src/g_heretic/a_hereticimp.cpp",
        "src/g_heretic/a_hereticweaps.cpp",
        "src/g_heretic/a_ironlich.cpp",
        "src/g_heretic/a_knight.cpp",
        "src/g_heretic/a_wizard.cpp",
        "src/g_hexen/a_bats.cpp",
        "src/g_hexen/a_bishop.cpp",
        "src/g_hexen/a_blastradius.cpp",
        "src/g_hexen/a_boostarmor.cpp",
        "src/g_hexen/a_centaur.cpp",
        "src/g_hexen/a_clericflame.cpp",
        "src/g_hexen/a_clericholy.cpp",
        "src/g_hexen/a_clericmace.cpp",
        "src/g_hexen/a_clericstaff.cpp",
        "src/g_hexen/a_dragon.cpp",
        "src/g_hexen/a_fighteraxe.cpp",
        "src/g_hexen/a_fighterhammer.cpp",
        "src/g_hexen/a_fighterplayer.cpp",
        "src/g_hexen/a_fighterquietus.cpp",
        "src/g_hexen/a_firedemon.cpp",
        "src/g_hexen/a_flechette.cpp",
        "src/g_hexen/a_flies.cpp",
        "src/g_hexen/a_fog.cpp",
        "src/g_hexen/a_healingradius.cpp",
        "src/g_hexen/a_heresiarch.cpp",
        "src/g_hexen/a_hexenspecialdecs.cpp",
        "src/g_hexen/a_iceguy.cpp",
        "src/g_hexen/a_korax.cpp",
        "src/g_hexen/a_magecone.cpp",
        "src/g_hexen/a_magelightning.cpp",
        "src/g_hexen/a_magestaff.cpp",
        "src/g_hexen/a_pig.cpp",
        "src/g_hexen/a_serpent.cpp",
        "src/g_hexen/a_spike.cpp",
        "src/g_hexen/a_summon.cpp",
        "src/g_hexen/a_teleportother.cpp",
        "src/g_hexen/a_wraith.cpp",
        "src/g_shared/sbarinfo_commands.cpp",
        "src/g_strife/a_acolyte.cpp",
        "src/g_strife/a_alienspectres.cpp",
        "src/g_strife/a_coin.cpp",
        "src/g_strife/a_crusader.cpp",
        "src/g_strife/a_entityboss.cpp",
        "src/g_strife/a_inquisitor.cpp",
        "src/g_strife/a_loremaster.cpp",
        "src/g_strife/a_oracle.cpp",
        "src/g_strife/a_programmer.cpp",
        "src/g_strife/a_reaver.cpp",
        "src/g_strife/a_rebels.cpp",
        "src/g_strife/a_sentinel.cpp",
        "src/g_strife/a_spectral.cpp",
        "src/g_strife/a_stalker.cpp",
        "src/g_strife/a_strifeitems.cpp",
        "src/g_strife/a_strifeweapons.cpp",
        "src/g_strife/a_templar.cpp",
        "src/g_strife/a_thingstoblowup.cpp",
        ":xlat_parser",
    ],
)

cc_library(
    name = "fmopl",
    srcs = glob([
        "src/oplsynth/*.h",
        "src/*.h",
    ]) + [
        "src/oplsynth/fmopl.cpp",
    ],
    copts = [
        "-Dstricmp=strcasecmp",
        "-Dstrnicmp=strncasecmp",
        "-fno-tree-dominator-opts",
        "-fno-tree-fre",
    ],
    includes = [
        "src",
    ],
)

cc_binary(
    name = "zipdir",
    srcs = [
        "tools/zipdir/zipdir.c",
    ],
    deps = [
        ":bzip2",
        ":lzma",
        "@zlib",
    ],
)

filegroup(
    name = "wadsrc",
    srcs = [
        "wadsrc/static",
    ],
)

genrule(
    name = "vizdoom_pk3",
    srcs = [":wadsrc"],
    outs = ["vizdoom.pk3"],
    cmd = "$(execpath zipdir) -udf $@ $<",
    tools = [":zipdir"],
    visibility = ["//visibility:public"],
)

cc_binary(
    name = "vizdoom",
    srcs = glob([
        "src/*.h",
        "src/**/*.h",
    ]) + [
        ":viz_version",
        ":sc_man_scanner",
        "src/asm_x86_64/tmap3.s",
        "src/__autostart.cpp",
        "src/x86.cpp",
        "src/actorptrselect.cpp",
        "src/am_map.cpp",
        "src/b_bot.cpp",
        "src/b_func.cpp",
        "src/b_game.cpp",
        "src/b_move.cpp",
        "src/b_think.cpp",
        "src/bbannouncer.cpp",
        "src/c_bind.cpp",
        "src/c_cmds.cpp",
        "src/c_console.cpp",
        "src/c_consolebuffer.cpp",
        "src/c_cvars.cpp",
        "src/c_dispatch.cpp",
        "src/c_expr.cpp",
        "src/cmdlib.cpp",
        "src/colormatcher.cpp",
        "src/compatibility.cpp",
        "src/configfile.cpp",
        "src/ct_chat.cpp",
        "src/d_dehacked.cpp",
        "src/d_iwad.cpp",
        "src/d_main.cpp",
        "src/d_net.cpp",
        "src/d_netinfo.cpp",
        "src/d_protocol.cpp",
        "src/decallib.cpp",
        "src/dobject.cpp",
        "src/dobjgc.cpp",
        "src/dobjtype.cpp",
        "src/doomdef.cpp",
        "src/doomstat.cpp",
        "src/dsectoreffect.cpp",
        "src/dthinker.cpp",
        "src/f_wipe.cpp",
        "src/farchive.cpp",
        "src/files.cpp",
        "src/g_doomedmap.cpp",
        "src/g_game.cpp",
        "src/g_hub.cpp",
        "src/g_level.cpp",
        "src/g_mapinfo.cpp",
        "src/g_skill.cpp",
        "src/gameconfigfile.cpp",
        "src/gi.cpp",
        "src/gitinfo.cpp",
        "src/hu_scores.cpp",
        "src/i_net.cpp",
        "src/info.cpp",
        "src/keysections.cpp",
        "src/lumpconfigfile.cpp",
        "src/m_alloc.cpp",
        "src/m_argv.cpp",
        "src/m_bbox.cpp",
        "src/m_cheat.cpp",
        "src/m_joy.cpp",
        "src/m_misc.cpp",
        "src/m_png.cpp",
        "src/m_random.cpp",
        "src/m_specialpaths.cpp",
        "src/memarena.cpp",
        "src/md5.cpp",
        "src/name.cpp",
        "src/posix/i_cd.cpp",
        "src/posix/i_movie.cpp",
        "src/posix/i_steam.cpp",
        "src/posix/sdl/crashcatcher.c",
        "src/posix/sdl/hardware.cpp",
        "src/posix/sdl/i_gui.cpp",
        "src/posix/sdl/i_input.cpp",
        "src/posix/sdl/i_joystick.cpp",
        "src/posix/sdl/i_main.cpp",
        "src/posix/sdl/i_system.cpp",
        "src/posix/sdl/i_timer.cpp",
        "src/posix/sdl/sdlvideo.cpp",
        "src/posix/sdl/st_start.cpp",
        "src/nodebuild.cpp",
        "src/nodebuild_classify_sse2.cpp",
        "src/nodebuild_classify_nosse2.cpp",
        "src/nodebuild_events.cpp",
        "src/nodebuild_extract.cpp",
        "src/nodebuild_gl.cpp",
        "src/nodebuild_utility.cpp",
        "src/pathexpander.cpp",
        "src/p_3dfloors.cpp",
        "src/p_3dmidtex.cpp",
        "src/p_acs.cpp",
        "src/p_buildmap.cpp",
        "src/p_ceiling.cpp",
        "src/p_conversation.cpp",
        "src/p_doors.cpp",
        "src/p_effect.cpp",
        "src/p_enemy.cpp",
        "src/p_floor.cpp",
        "src/p_glnodes.cpp",
        "src/p_interaction.cpp",
        "src/p_lights.cpp",
        "src/p_linkedsectors.cpp",
        "src/p_lnspec.cpp",
        "src/p_map.cpp",
        "src/p_maputl.cpp",
        "src/p_mobj.cpp",
        "src/p_pillar.cpp",
        "src/p_plats.cpp",
        "src/p_pspr.cpp",
        "src/p_saveg.cpp",
        "src/p_sectors.cpp",
        "src/p_setup.cpp",
        "src/p_sight.cpp",
        "src/p_slopes.cpp",
        "src/p_spec.cpp",
        "src/p_states.cpp",
        "src/p_switch.cpp",
        "src/p_tags.cpp",
        "src/p_teleport.cpp",
        "src/p_terrain.cpp",
        "src/p_things.cpp",
        "src/p_tick.cpp",
        "src/p_trace.cpp",
        "src/p_udmf.cpp",
        "src/p_usdf.cpp",
        "src/p_user.cpp",
        "src/p_writemap.cpp",
        "src/p_xlat.cpp",
        "src/parsecontext.cpp",
        "src/po_man.cpp",
        "src/r_swrenderer.cpp",
        "src/r_utility.cpp",
        "src/r_3dfloors.cpp",
        "src/r_bsp.cpp",
        "src/r_draw.cpp",
        "src/r_drawt.cpp",
        "src/r_main.cpp",
        "src/r_plane.cpp",
        "src/r_segs.cpp",
        "src/r_sky.cpp",
        "src/r_things.cpp",
        "src/s_advsound.cpp",
        "src/s_environment.cpp",
        "src/s_playlist.cpp",
        "src/s_sndseq.cpp",
        "src/s_sound.cpp",
        "src/sc_man.cpp",
        "src/st_stuff.cpp",
        "src/statistics.cpp",
        "src/stats.cpp",
        "src/stringtable.cpp",
        "src/strnatcmp.c",
        "src/tables.cpp",
        "src/teaminfo.cpp",
        "src/tempfiles.cpp",
        "src/v_blend.cpp",
        "src/v_collection.cpp",
        "src/v_draw.cpp",
        "src/v_font.cpp",
        "src/v_palette.cpp",
        "src/v_pfx.cpp",
        "src/v_text.cpp",
        "src/v_video.cpp",
        "src/w_wad.cpp",
        "src/wi_stuff.cpp",
        "src/zstrformat.cpp",
        "src/zstring.cpp",
        "src/GuillotineBinPack.cpp",
        "src/SkylineBinPack.cpp",
        "src/g_doom/a_doommisc.cpp",
        "src/g_heretic/a_hereticmisc.cpp",
        "src/g_hexen/a_hexenmisc.cpp",
        "src/g_raven/a_artitele.cpp",
        "src/g_raven/a_minotaur.cpp",
        "src/g_strife/a_strifestuff.cpp",
        "src/g_strife/strife_sbar.cpp",
        "src/g_shared/a_action.cpp",
        "src/g_shared/a_armor.cpp",
        "src/g_shared/a_artifacts.cpp",
        "src/g_shared/a_bridge.cpp",
        "src/g_shared/a_camera.cpp",
        "src/g_shared/a_debris.cpp",
        "src/g_shared/a_decals.cpp",
        "src/g_shared/a_fastprojectile.cpp",
        "src/g_shared/a_flashfader.cpp",
        "src/g_shared/a_fountain.cpp",
        "src/g_shared/a_hatetarget.cpp",
        "src/g_shared/a_keys.cpp",
        "src/g_shared/a_lightning.cpp",
        "src/g_shared/a_mapmarker.cpp",
        "src/g_shared/a_morph.cpp",
        "src/g_shared/a_movingcamera.cpp",
        "src/g_shared/a_pickups.cpp",
        "src/g_shared/a_puzzleitems.cpp",
        "src/g_shared/a_quake.cpp",
        "src/g_shared/a_randomspawner.cpp",
        "src/g_shared/a_secrettrigger.cpp",
        "src/g_shared/a_sectoraction.cpp",
        "src/g_shared/a_setcolor.cpp",
        "src/g_shared/a_skies.cpp",
        "src/g_shared/a_soundenvironment.cpp",
        "src/g_shared/a_soundsequence.cpp",
        "src/g_shared/a_spark.cpp",
        "src/g_shared/a_specialspot.cpp",
        "src/g_shared/a_waterzone.cpp",
        "src/g_shared/a_weaponpiece.cpp",
        "src/g_shared/a_weapons.cpp",
        "src/g_shared/hudmessages.cpp",
        "src/g_shared/sbarinfo.cpp",
        "src/g_shared/sbar_mugshot.cpp",
        "src/g_shared/shared_hud.cpp",
        "src/g_shared/shared_sbar.cpp",
        "src/intermission/intermission.cpp",
        "src/intermission/intermission_parse.cpp",
        "src/menu/colorpickermenu.cpp",
        "src/menu/joystickmenu.cpp",
        "src/menu/listmenu.cpp",
        "src/menu/loadsavemenu.cpp",
        "src/menu/menu.cpp",
        "src/menu/menudef.cpp",
        "src/menu/menuinput.cpp",
        "src/menu/messagebox.cpp",
        "src/menu/optionmenu.cpp",
        "src/menu/playerdisplay.cpp",
        "src/menu/playermenu.cpp",
        "src/menu/readthis.cpp",
        "src/menu/videomenu.cpp",
        "src/oplsynth/mlopl.cpp",
        "src/oplsynth/mlopl_io.cpp",
        "src/oplsynth/music_opldumper_mididevice.cpp",
        "src/oplsynth/music_opl_mididevice.cpp",
        "src/oplsynth/opl_mus_player.cpp",
        "src/oplsynth/dosbox/opl.cpp",
        "src/oplsynth/OPL3.cpp",
        "src/oplsynth/nukedopl3.cpp",
        "src/resourcefiles/ancientzip.cpp",
        "src/resourcefiles/file_7z.cpp",
        "src/resourcefiles/file_grp.cpp",
        "src/resourcefiles/file_lump.cpp",
        "src/resourcefiles/file_rff.cpp",
        "src/resourcefiles/file_wad.cpp",
        "src/resourcefiles/file_zip.cpp",
        "src/resourcefiles/file_pak.cpp",
        "src/resourcefiles/file_directory.cpp",
        "src/resourcefiles/resourcefile.cpp",
        "src/sfmt/SFMT.cpp",
        "src/sound/fmodsound.cpp",
        "src/sound/i_music.cpp",
        "src/sound/i_sound.cpp",
        "src/sound/mpg123_decoder.cpp",
        "src/sound/music_cd.cpp",
        "src/sound/music_dumb.cpp",
        "src/sound/music_gme.cpp",
        "src/sound/music_mus_midiout.cpp",
        "src/sound/music_smf_midiout.cpp",
        "src/sound/music_hmi_midiout.cpp",
        "src/sound/music_xmi_midiout.cpp",
        "src/sound/music_midistream.cpp",
        "src/sound/music_midi_base.cpp",
        "src/sound/music_midi_timidity.cpp",
        "src/sound/music_mus_opl.cpp",
        "src/sound/music_stream.cpp",
        "src/sound/music_fluidsynth_mididevice.cpp",
        "src/sound/music_softsynth_mididevice.cpp",
        "src/sound/music_timidity_mididevice.cpp",
        "src/sound/music_wildmidi_mididevice.cpp",
        "src/sound/music_win_mididevice.cpp",
        "src/sound/oalsound.cpp",
        "src/sound/sndfile_decoder.cpp",
        "src/sound/music_pseudo_mididevice.cpp",
        "src/textures/animations.cpp",
        "src/textures/anim_switches.cpp",
        "src/textures/automaptexture.cpp",
        "src/textures/bitmap.cpp",
        "src/textures/buildtexture.cpp",
        "src/textures/canvastexture.cpp",
        "src/textures/ddstexture.cpp",
        "src/textures/flattexture.cpp",
        "src/textures/imgztexture.cpp",
        "src/textures/jpegtexture.cpp",
        "src/textures/multipatchtexture.cpp",
        "src/textures/patchtexture.cpp",
        "src/textures/pcxtexture.cpp",
        "src/textures/pngtexture.cpp",
        "src/textures/rawpagetexture.cpp",
        "src/textures/emptytexture.cpp",
        "src/textures/texture.cpp",
        "src/textures/texturemanager.cpp",
        "src/textures/tgatexture.cpp",
        "src/textures/warptexture.cpp",
        "src/thingdef/olddecorations.cpp",
        "src/thingdef/thingdef.cpp",
        "src/thingdef/thingdef_codeptr.cpp",
        "src/thingdef/thingdef_data.cpp",
        "src/thingdef/thingdef_exp.cpp",
        "src/thingdef/thingdef_expression.cpp",
        "src/thingdef/thingdef_function.cpp",
        "src/thingdef/thingdef_parse.cpp",
        "src/thingdef/thingdef_properties.cpp",
        "src/thingdef/thingdef_states.cpp",
        "src/timidity/common.cpp",
        "src/timidity/instrum.cpp",
        "src/timidity/instrum_dls.cpp",
        "src/timidity/instrum_font.cpp",
        "src/timidity/instrum_sf2.cpp",
        "src/timidity/mix.cpp",
        "src/timidity/playmidi.cpp",
        "src/timidity/resample.cpp",
        "src/timidity/timidity.cpp",
        "src/wildmidi/file_io.cpp",
        "src/wildmidi/gus_pat.cpp",
        "src/wildmidi/reverb.cpp",
        "src/wildmidi/wildmidi_lib.cpp",
        "src/wildmidi/wm_error.cpp",
        "src/xlat/parse_xlat.cpp",
        "src/fragglescript/t_fspic.cpp",
        "src/fragglescript/t_func.cpp",
        "src/fragglescript/t_load.cpp",
        "src/fragglescript/t_oper.cpp",
        "src/fragglescript/t_parse.cpp",
        "src/fragglescript/t_prepro.cpp",
        "src/fragglescript/t_script.cpp",
        "src/fragglescript/t_spec.cpp",
        "src/fragglescript/t_variable.cpp",
        "src/fragglescript/t_cmd.cpp",
        "src/r_data/colormaps.cpp",
        "src/r_data/sprites.cpp",
        "src/r_data/voxels.cpp",
        "src/r_data/renderstyle.cpp",
        "src/r_data/r_interpolate.cpp",
        "src/r_data/r_translate.cpp",
        "src/zzautozend.cpp",
        "src/viz_buffers.cpp",
        "src/viz_depth.cpp",
        "src/viz_game.cpp",
        "src/viz_input.cpp",
        "src/viz_labels.cpp",
        "src/viz_main.cpp",
        "src/viz_message_queue.cpp",
        "src/viz_shared_memory.cpp",
        "src/viz_system.cpp",
    ],
    copts = [
        "-Dstricmp=strcasecmp",
        "-Dstrnicmp=strncasecmp",
        "-DNO_GTK=1",
        "-DNO_FMOD=1",
        "-DNO_OPENAL=1",
        "-std=c++11",
        "-fPIC",
        "-fomit-frame-pointer",
        "-D__forceinline=inline",
        "-Wno-unused-result",
        "-Wall",
        "-Wno-unused",
        "-Wno-unused-parameter",
        "-Wno-missing-field-initializers",
        "-Wno-class-memaccess",
        "-Wno-misleading-indentation",
        "-Wno-implicit-fallthrough",
        "-Wno-conversion-null",
        "-Wno-deprecated-copy",
        "-Wno-stringop-truncation",
        "-Wno-cast-function-type",
        "-DBACKPATCH",
        "-msse",
        "-msse2",
        "-mmmx",
    ],
    data = [
        ":vizdoom_pk3",
    ],
    includes = [
        "src",
        "src/g_doom",
        "src/g_hexen",
        "src/g_raven",
        "src/g_shared",
        "src/g_strife",
        "src/posix",
        "src/posix/sdl",
        "src/sound",
        "src/textures",
        "src/thingdef",
        "src/xlat",
    ],
    linkopts = [
        "-lpthread",
        "-lrt",
        "-ldl",
    ],
    linkstatic = 1,
    visibility = ["//visibility:public"],
    deps = [
        ":bzip2",
        ":dumb",
        ":fmopl",
        ":gdtoa",
        ":gme",
        ":headers",
        ":lzma",
        "@boost//:chrono",
        "@boost//:interprocess",
        "@boost//:thread",
        "@libjpeg_turbo//:jpeg",
        "@sdl2",
        "@zlib",
    ],
)
