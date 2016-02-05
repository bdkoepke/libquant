cmake_minimum_required(VERSION 2.8)
include(noweb.cmake)
include(UseLatex)
set(quant_C
    binomial/one_period.c)
set(quant_H
    binomial/one_period.h)
set(quant_TEX
    binomial/one_period.tex)

tangle("${quant_H}")
tangle("${quant_C}")
weave("${quant_TEX}")
add_custom_target(tangle DEPENDS ${quant_H} ${quant_C})
add_custom_target(weave DEPENDS ${quant_TEX})

add_executable(quant ${quant_C})
add_dependencies(quant tangle)
target_link_libraries(quant m)

add_latex_document(libquant.tex
                   INPUTS binomial/introduction.tex preamble/preamble.tex preamble/noweb.tex preamble/theorem.tex
                   FORCE_PDF
                   TARGET_NAME libquant)
add_dependencies(libquant_pdf weave)
